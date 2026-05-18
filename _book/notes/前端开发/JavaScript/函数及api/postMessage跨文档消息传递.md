# postMessage跨文档消息传递

```window.postMessage()``` 方法允许来自一个文档的脚本可以传递文本消息到另一个文档里的脚本，而不用管是否跨域，可以用这种消息传递技术来实现安全的通信。这项技术称为“**跨文档消息传递**”，又称为“**窗口间消息传递**”或者“**跨域消息传递**”。

## **用法：**
#### **发送消息**
```
otherWindow.postMessage(message, targetOrigin, [transfer]);
```

**otherWindow**：其他窗口的一个引用。比如 iframe 的 contentWindow 属性、执行 window.open 返回的窗口对象、或者是命名过或数值索引的 window.frames。

**message** ：要发送的消息。它将会被结构化克隆算法序列化，所以无需自己序列化，html5规范中提到该参数可以是JavaScript的任意基本类型或可复制的对象，然而并不是所有浏览器都做到了这点儿，部分浏览器只能处理字符串参数，所以我们在传递参数的时候需要使用JSON.stringify()方法对对象参数序列化。

#### **接收消息**
如果指定的源匹配的话，那么当调用 postMessage() 方法的时候，在目标窗口的Window对象上就会触发一个 message 事件。 获取postMessage传来的消息：为页面添加onmessage事件。

```
window.addEventListener('message',function(e) {
   var origin = event.origin;
   // 通常，onmessage()事件处理程序应当首先检测其中的origin属性，忽略来自未知源的消息
   if (origin !== "http://example.org:8080") return;
   // ...
}, false)
```