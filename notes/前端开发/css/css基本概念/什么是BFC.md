# 什么是BFC

## **背景知识：**
CSS中有三种常见的定位方案：
 - **普通流 (normal flow)**

	在普通流中，元素按照其在 HTML 中的先后位置至上而下布局，在这个过程中，行内元素水平排列，直到当行被占满然后换行，块级元素则会被渲染为完整的一个新行，除非另外指定，否则所有元素默认都是普通流定位，也可以说，普通流中元素的位置由该元素在 HTML 文档中的位置决定。
 -  **浮动 (float)**

	在浮动布局中，元素首先按照普通流的位置出现，然后根据浮动的方向尽可能的向左边或右边偏移，其效果与印刷排版中的文本环绕相似。
 -  **绝对定位 (absolute positioning)**
    
    在绝对定位布局中，元素会整体脱离普通流，因此绝对定位元素不会对其兄弟元素造成影响，而元素具体的位置由绝对定位的坐标决定。

## **BFC概念：**
BFC 即 Block Formatting Contexts (块级格式化上下文)，它属于上述定位方案的普通流。
具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。
通俗一点来讲，可以把 BFC 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。

## **BFC触发条件**

只要元素满足下边任意一个条件，那么这个元素就触发了BFC:
 - overflow: hidden、auto、scroll
 - display: inline-block、table-cells、flex
 - position: absolute、fixed
 - body根元素
 - float：除了none以外的其他值

## **BFC纵向重叠问题：**
这个是BFC的一个规则，就是属于同一个BFC的两个相邻标签的纵向外边距会重叠，但是横向边距不会出现重叠，代码举例：

JSX：
```
function App() {
    return(
        <div class="container">
            <div></div>
            <div></div>
        </div>
    )
}
```
CSS:
```
.container {
  width: 400px;
  height: 200px;
  background-color: blanchedalmond;
}
.container > div:first-child {
  background-color: blueviolet;
  width: 100%;
  height: 100px;
  margin-bottom: 100px;
}
.container > div:nth-child(2) {
  background-color: aquamarine;
  width: 100%;
  height: 50px;
  margin-top: 50px;
}
```

<div align="center">
    <img src=./什么是BFC1.png width=40% />
    <img src=./什么是BFC2.png width=40% />
</div>

**图片注释：** 深一点的咖色表示margin区域，可以看到margin之间会重叠

如果改成横向排布，左右边距之间不会发生重叠，新的css如下：
```
.container {
  width: 400px;
  height: 200px;
  background-color: blanchedalmond;
  display: flex;
}
.container > div:first-child {
  background-color: blueviolet;
  width: 100%;
  height: 100px;
  margin-right: 100px;
}
.container > div:nth-child(2) {
  background-color: aquamarine;
  width: 100%;
  height: 50px;
  margin-left: 50px;
}
```
<div align="center">
    <img src=./什么是BFC3.png width=40% />
    <img src=./什么是BFC4.png width=40% />
</div>