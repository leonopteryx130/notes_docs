# react挂载元素根组件标记

特性：react不允许同一个dom元素上挂载多个根组件。

说到这个特性的实现原理，与两个方法密切相关，文件路径```/react-dom-bindings/src/client/ReactDOMComponentTree.js```，方法源码：

```
export function markContainerAsRoot(hostRoot: Fiber, node: Container): void {
  // $FlowFixMe[prop-missing]
  node[internalContainerInstanceKey] = hostRoot;
}

export function isContainerMarkedAsRoot(node: Container): boolean {
  // $FlowFixMe[prop-missing]
  return !!node[internalContainerInstanceKey];
}
```

markContainerAsRoot方法有两个输入参数，第一个参数暂时可以不用管，可以简单理解为一个值就好，第二个参数就是根组件挂载到的html节点，看变量类型命名，react团队是希望把这个节点称为容器。

isContainerMarkedAsRoot方法只接受一个容器作为输入参数，返回一个布尔值。

这两个方法是一对方法，markContainerAsRoot给容器对象添加一个属性，并赋值，用于给挂载了根组件的html节点加一个标识。isContainerMarkedAsRoot方法用于检测这个唯一标识，如果存在这个属性，那么返回true，如果不存在返回false。

唯一标识变量internalContainerInstanceKey作为节点属性key，要确保它是唯一的，react的实现方式是：

```
const randomKey = Math.random().toString(36).slice(2);
const internalInstanceKey = '__reactFiber$' + randomKey;
```
也就是说一个字符串，以__reactFiber$开头，后边是随机的。

再看createRoot方法中（源码路径```/packages/react-dom/src/client/ReactDOMRoot.js```），在创建根组件之前就调用了下边这个方法：

```
warnIfReactDOMContainerInDEV(container);
```

warnIfReactDOMContainerInDEV的if语句判断条件就包含isContainerMarkedAsRoot方法，createReact也正是通过这个方法来检测dom元素是否重复挂载根组件。同样，在根组件创建完（createRoot中的root变量）后，也是立即调用了markContainerAsRoot方法，对html元素进行了一个标记