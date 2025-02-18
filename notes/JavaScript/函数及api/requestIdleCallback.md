# requestIdleCallback

### 函数签名

```js
requestIdleCallback(callback[, options])
```

### 参数

- `callback`：回调函数，在浏览器空闲时期被调用。回调函数会接收到一个 IdleDeadline 对象作为参数，该对象包含 `didTimeout`、`timeRemaining()` 两个属性。didTimeout 是一个boolean值，表示回调函数是否由于超时而被调用的。timeRemaining() 返回当前空闲周期的剩余时间，单位为毫秒。
- `options`：可选参数，目前只有一个属性 `timeout`，表示超过这个时间，回调函数必须被执行。

### 返回值

返回一个 ID，可以传递给 `cancelIdleCallback` 以取消回调函数。

### 作用

`requestIdleCallback` 是一个用于在浏览器空闲时期执行任务的 API，即task queue为空的时候执行的，这相当于是安排了一个低优先级的任务。它可以帮助开发者更好地利用[浏览器的空闲时间](../../计算机科学/浏览器/浏览器的空闲时间.md)，从而提高网页的性能和用户体验。

一个简单的demo，在react框架下，结合```requestAnimationFrame``` api 打印出每一帧的剩余时间，同时提供一个按钮改变dom结构，观察浏览器空闲时间的变化。

```jsx
import React, { useEffect, useCallback, useRef, useState } from 'react';

const App = () => {
  const idleCallbackRef = useRef(null);
  const [innerHtmlText, setInner] = useState(['My Component']);

  const handleIdleCallback = useCallback((IdleDeadline) => {
    console.log('剩余空闲时间', IdleDeadline.timeRemaining(), 'ms');
  }, []);

  useEffect(() => {
    const handleAnimationFrame = () => {
      idleCallbackRef.current = window.requestIdleCallback(handleIdleCallback, { timeout: 10000 });
      window.requestAnimationFrame(handleAnimationFrame);
    };

    handleAnimationFrame();

    return () => {
      if (idleCallbackRef.current) {
        window.cancelIdleCallback(idleCallbackRef.current);
      }
    };
  }, [handleIdleCallback]);

  return (
    <div>
      {innerHtmlText.map((item, index) => (
        <div key={index}>{item}</div>
      ))}
      <button onClick={() => {
        setInner([...innerHtmlText, ...innerHtmlText]);
      }}>Click</button>
    </div>
  );
};

export default App;
```


