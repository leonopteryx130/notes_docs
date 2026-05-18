# 为什么有两个createRoot方法？

在文件路径```/packages/react-dom/src/client/ReactDOM.js```和```/packages/react-dom/src/client/ReactDOMRoot.js```中都包含有createRoot方法

实际项目中的createRoot方法是通过```/packages/react-dom/index.js```这个文件引入的，源码：

```
export {
  ...
  createRoot,
  ...
} from './src/client/ReactDOM';
```

找到ReactDOM.js文件的相关代码：

```
import {
  createRoot as createRootImpl,
  hydrateRoot as hydrateRootImpl,
  isValidContainer,
} from './ReactDOMRoot';

function createRoot(
  container: Element | Document | DocumentFragment,
  options?: CreateRootOptions,
): RootType {
  ...
  return createRootImpl(container, options);
}
```

可以看到是返回了一个createRootImpl方法，这个方法是ReactDOMRoot文件中同名方法的别名。相当于在ReactDOM文件中对原有的createRoot方法添加了一些内容，再暴露出去，这样写的好处是新添加的功能插拔灵活，并且如果这个方法是多人共同编辑的，代码逻辑上可以做到互不影响。