# SharedWorker

### 前置概念

**Worker**：HTML5 引入的后台线程机制，让 JavaScript 可以在主线程之外运行，避免复杂计算阻塞页面渲染。

普通 Worker（Dedicated Worker）是「一对一」的：一个页面创建一个 Worker，只有这个页面能和它通信。

### 基本概念

**SharedWorker**，即可共享的 Worker。同源（协议、域名、端口都相同）下的多个标签页、窗口，都能连接到**同一个** SharedWorker 实例，实现数据互通。

它运行在独立于所有页面主线程的后台线程中：既不会阻塞页面，又能统一处理多页面的请求。

简单来说，SharedWorker 像一个「公共服务端」，多个页面作为「客户端」与之建立连接，通过它完成数据传递与协同。和普通 Worker 的「一对一」不同，它是「一对多」的通信方案。

### 通信结构

多个页面连同一个 SharedWorker，各自通过独立的 `MessagePort` 收发消息；Worker 内部常用 `connections` Map，按页面 id 保存各端 port，方便定向转发。对照下图：

<div align="center">
    <img src=./shareWorker1.png width=80% />
</div>

完整链路可以按四步理解：

1. **创建实例**：页面执行 `new SharedWorker('./share.js')`。若该脚本还没有对应的 SharedWorker，浏览器会启动一个后台线程并加载脚本；已有则直接复用，脚本始终是共享的那一份。
2. **端口建立**：每次连接都会得到一条独立的 `MessagePort`，作为该页面与 Worker 之间的专用通道。
3. **数据传递**：双方用 `port.postMessage` 发消息、用 `port.onmessage` 收消息。Worker 处理完后，可回给单个页面，也可广播给所有已连接页面。
4. **实例销毁**：所有连接该 SharedWorker 的页面都关闭后，后台线程才会被浏览器销毁并释放资源。

### 代码 demo

代码分为两部分：页面代码和 Worker 代码。流程如下：

1. 页面执行 `new SharedWorker('./share.js')`
2. Worker 触发 `onconnect`，从 `e.ports[0]` 取出 port，建立连接
3. Worker 在该 port 上注册 `onmessage`
4. 页面用 `worker.port` 注册 `onmessage`；若用的是 `addEventListener('message', ...)`，还需调用 `port.start()`（用 `onmessage = ...` 时会自动 start）
5. 页面通过 `port.postMessage` 发消息
6. Worker 收到后处理，可回给单个页面，也可广播给所有已连接页面

**Worker 代码（`share.js`）**

```js
const connections = new Map();

self.onconnect = (e) => {
  const port = e.ports[0];

  port.onmessage = (event) => {
    const { type, pageId, text } = event.data;

    if (type === 'join') {
      connections.set(pageId, port);
      port.postMessage({ type: 'system', text: `已连接，当前共 ${connections.size} 个页面` });
      return;
    }

    if (type === 'chat') {
      // 广播给所有已连接页面
      for (const [id, p] of connections) {
        p.postMessage({ type: 'chat', from: pageId, text });
      }
    }
  };
};
```

**页面代码（可开两个标签页验证互通）**

```js
const pageId = 'page-' + Math.random().toString(36).slice(2, 8);
const worker = new SharedWorker('./share.js');
const port = worker.port;

port.onmessage = (e) => {
  const data = e.data;
  if (data.type === 'system') {
    console.log(`[系统] ${data.text}`);
  } else if (data.type === 'chat') {
    console.log(`[${data.from}] ${data.text}`);
  }
};

// 建立连接后先上报自己的 pageId
port.postMessage({ type: 'join', pageId });

// 发送消息（示例）
port.postMessage({ type: 'chat', pageId, text: 'hello from ' + pageId });
```

### 工程经验

实际项目里建议把消息类型拆开：用 `broadcast` 做广播，用 `private` 做点对点。Worker 按 `type` 分发即可，页面侧只负责发消息、收结果。

Worker 分发示例：

```js
port.onmessage = (event) => {
  const { type, pageId, to, text } = event.data;

  if (type === 'join') {
    connections.set(pageId, port);
    return;
  }

  // 广播：发给所有已连接页面
  if (type === 'broadcast') {
    for (const p of connections.values()) {
      p.postMessage({ type: 'broadcast', from: pageId, text });
    }
    return;
  }

  // 点对点：只发给指定页面
  if (type === 'private') {
    const target = connections.get(to);
    if (target) {
      target.postMessage({ type: 'private', from: pageId, text });
    }
  }
};
```

页面发送示例：

```js
// 广播
port.postMessage({ type: 'broadcast', pageId, text: '全员通知' });

// 点对点（to 为目标页面 id）
port.postMessage({ type: 'private', pageId, to: 'page-456', text: '只给你看' });
```

