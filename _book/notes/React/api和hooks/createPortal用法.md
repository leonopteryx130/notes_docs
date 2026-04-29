# createPortal

### 概述：

> createPortal 允许你将 JSX 作为 children 渲染至 DOM 的不同部分。

### api:

```
<div>
  <SomeComponent />
  {createPortal(children, domNode, key?)}
</div>
```

### 参数

- children：React 可以渲染的任何内容，如 JSX 片段、字符串或数字，以及这些内容构成的数组。
- domNode：某个已经存在的 DOM 节点，例如由 document.getElementById() 返回的节点。在更新过程中传递不同的 DOM 节点将导致 portal 内容被重建。

### demo

```javascript
import { createPortal } from 'react-dom';

function MyComponent() {
  return (
    <div style={{ border: '2px solid black' }}>
      <p>这个子节点被放置在父节点 div 中。</p>
      {createPortal(
        <p>这个子节点被放置在 document body 中。</p>,
        document.body
      )}
    </div>
  );
}
```

### 渲染出的dom

```html
<body>
  <div id="root">
    ...
      <div style="border: 2px solid black">
        <p>这个子节点被放置在父节点 div 中。</p>
      </div>
    ...
  </div>
  <p>这个子节点被放置在 document body 中。</p>
</body>
```
