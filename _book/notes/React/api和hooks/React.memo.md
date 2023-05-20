# React.memo

该api使得组件仅在它的 props 发生改变的时候进行重新渲染。通常来说，在组件树中 React 组件，只要有变化就会走一遍渲染流程。但是通过React.memo()，我们可以仅仅让某些组件进行渲染。

使用方法：
可以在export组件的时候使用：
```
import React from 'react';
const Button = ({ onClickButton, children }) => {
  return (
    <>
      <button onClick={onClickButton}>{children}</button>
      <span>{Math.random()}</span>
    </>
  );
};
export default React.memo(Button);
```

只有当输入的参数（onClickButton和children）有变化的时候，组件才会触发重新渲染，这样可以优化性能，避免渲染过多。

也可以在定义组件的时候使用：

```
import {memo} from 'react';
const Child = memo((props) => {
  console.log('child update')
  return (
  <div>
    {props.text}
  </div>
  )
})
```