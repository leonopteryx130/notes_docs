# createRoot节点类型和节点挂载检查

react18默认使用createRoot api创建根节点，createRoot方法中使用了两个api：```isValidContainerLegacy```和```warnIfReactDOMContainerInDEV```。
截选源码如下：

```
export function createRoot(
  container: Element | Document | DocumentFragment,
  options?: CreateRootOptions,
): RootType {
  if (!isValidContainer(container)) {
    throw new Error('...');
  }
  warnIfReactDOMContainerInDEV(container);
  ...
}
```

这两个api分别用来判断容器节点类型和节点挂载位置是否符合规范。

isValidContainerLegacy方法，源码：

```
export function isValidContainerLegacy(node: any): boolean {
  return !!(
    node &&
    (node.nodeType === ELEMENT_NODE ||
      node.nodeType === DOCUMENT_NODE ||
      node.nodeType === DOCUMENT_FRAGMENT_NODE ||
      (node.nodeType === COMMENT_NODE &&
        (node: any).nodeValue === ' react-mount-point-unstable '))
  );
}
```

解释：return值使用两个惊叹号表示将返回值转化为boolean类型返回。这个方法表示createRoot方法支持挂载的节点类型为一般元素，document，注释，如果挂载到注释的话，注释内容必须是```react-mount-point-unstable```

warnIfReactDOMContainerInDEV方法，源码比较长，就不写在文档里边了，这里边主要是对根节点的挂载位置做了一些处理，需要说明的是，如果挂载位置不符合预期，也不会中断运行，但是会在控制台抛出一个错误用于警告。具体是做了这些规定：

- 不建议直接将根节点挂载在document.body上
- 不支持根节点重复挂载，也就是说如果一个元素已经被挂载了根节点，那么就不能再挂载一个新的根节点了