# react自动批处理

在react17的时候，就默认会将多个setState合并，从而减少重新渲染的次数，但是react17仅支持在同步代码中合并setState，异步任务中依然存在多次渲染的情况，react18正式在异步任务中加入合并setState的机制。这种机制叫做自动批处理

React 会将之前未做处理的类似异步任务进行一个合并以减少渲染。主要合并途径有两种

- 对于 Promise，setTimeout、等基础异步任务中的多个 state 变更，进行自动合并。
- 对于基于 createRoot 创建出来的组件，其下的所有状态都会应用批量更新。

以下四种写法中，react都只会重新渲染一次：

```
// example 1
function handleClick() {
    setCount((c) => c + 1);
    setFlag((f) => !f);
}

// example 2
setTimeout(() => {
    setCount((c) => c + 1);
    setFlag((f) => !f);
}, 1000);

// example 3
fetch(/*...*/).then(() => {
    setCount((c) => c + 1);
    setFlag((f) => !f);
  });

// example 4
elm.addEventListener("click", () => {
    setCount((c) => c + 1);
    setFlag((f) => !f);
});
```