# react的两种启动方式

在React官方文档中介绍了三种启动方式，Legacy，Blocking，Concurrent。官方文档原文：
<div align="center">
    <img src=./react的两种启动方式1.png width=90% />
</div>
在react17.0.2中，Block模式已经被弃用了，源码中直接删除了这种方法，在React18中，默认使用Concurrent模式。

React18的root节点是通过createRoot这个方法创建的，这个方法的定义在源码文件```packages/react-dom/src/client/ReactDOMRoot.js```，在client目录下可以看到有这两种启动方式所定义的js文件：
<div align="left">
    <img src=./react的两种启动方式2.png width=30% />
</div>
createRoot方法中定义了一个root常量，初始化常量的时候用了createContainer方法，传入的第二个参数就是ConcurrentRoot：

```
//定义root的源码
const root = createContainer(
  container,
  ConcurrentRoot,
  null,
  isStrictMode,
  concurrentUpdatesByDefaultOverride,
  identifierPrefix,
  onRecoverableError,
  transitionCallbacks,
);
```
这个值是一个常量。常量的定义在```react-reconciler/src/ReactRootTags```路径下，这个文件只有三行代码：

```
export type RootTag = 0 | 1;
export const LegacyRoot = 0;
export const ConcurrentRoot = 1;
```
定义了如果使用Legacy启动模式，那么记录tag值为0，如果使用Concurrent启动模式，那么记录tag值为1

同样，在Legacy启动模式的js文件```packages/react-dom/src/client/ReactDOMLegacy.js```中legacyCreateRootFromDOMContainer方法中的root常量，执行初始化函数createHydrationContainer的时候，传入了一个LegacyRoot的参数
