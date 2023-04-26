# Fiber树

## **背景知识：**
DOM树，比如下边这段html片段：
```
<div>
    <ul>
        <li>1</li>
        <li>2</li>
    </ul>
    <button>submit</button>
<div>
```

对应的DOM树：
<div align="center">
    <img src=./Fiber树1.png width=70% />
</div>
React为了提高性能，使用了虚拟DOM，虚拟DOM允许在JS对象中进行新老节点对比，在React中，虚拟DOM对应的就是Fiber树。Fiber树和DOM树的结构稍有不同，因此遍历方式也会不一样，背景知识里边那段html对应的Fiber树如下：
<div align="center">
    <img src=./Fiber树2.png width=70% />
</div>
在Fiber树中，父节点只和它的第一个孩子节点相连接，而第一个孩子和后面的兄弟节点相连接，它们之间构成了一个单项链表的结构。最后，每个孩子都有一个指向父节点的指针。

Fiber树的遍历顺序:
- 把当前遍历的节点名记作 a
- 遍历当前节点 a，完成对这个节点要做的事
- 判断 a.child是否为空
- 若 a.child不为空，则把 a.child记作 a，回到 [步骤 1]
- 若 a.child为空，则判断 a.sibing是否为空，不为空将 a.sibing记为 a，回到步骤 1
- 若 a.child、a.sibling都为空，则证明当前节点和和他兄弟节点都遍历完了，那就返回它的父节点，找父节点中还没有遍历的兄弟节点，找到了，回到步骤 1
如此反复，直到遍历到顶点，结束。