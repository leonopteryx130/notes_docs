# linear-gradient

CSS linear-gradient() 函数用于创建一个表示两种或多种颜色线性渐变的图片，在css的background属性后使用。

语法：

```
linear-gradient( [ <angle> | to <side-or-corner> ,]? <color-stop> [, <color-stop>]+ )
```

这个语法结构看起来比较复杂，也容易产生迷惑，其实参数本质是由两部分组成： ```<angle> | to <side-or-corner>```和```<color-stop> [, <color-stop>]+```

- ```<angle> | to <side-or-corner>```用于描述渐变方向
- ```<color-stop> [, <color-stop>]+```用于描述具体的渐变颜色

渐变方向有两种表示方法，一种是角度描述（```<angle>```，deg语法），另一种是方向描述（```to <side-or-corner>```，to语法）。
deg语法示例：

```
background: linear-gradient(45deg, black, white);
```
表示按顺时针45°角（也就是从左下角到右上角）从黑到白。
to语法示例：

```
background: linear-gradient(to left, black, white);
```
to表示指针的方向，表示从右到左，从黑到白。

等价于deg语法：

```
background: linear-gradient(270deg, black, white);
```
如果to后边是一个方向表示side模式，也可以添加两个方向表示corner模式，举个例子：

```
background: linear-gradient(to left bottom, black, white);
```

表示从右上到左下，从黑到白。
**注意**：to是到的意思，渐变方向非必须的，可以没有

颜色的表示有两个核心要素，颜色和“位置”，位置通常用百分比分段表示，每一段之间用逗号隔开。举个例子：

```
background: linear-gradient(red 40%, yellow 30%, blue 65%);
```
表示位置的百分比通常是递增的，但是如果后边一个位置小于前边一个位置，像上边例子一样，颜色之间就会出现一个硬切换，上边例子的效果是从红色到黄色在 40% 的位置，然后过渡从黄色到蓝色终止于 65% 的位置处。

<div align="center">
    <img src=./linear-gradient.png width=60% />
</div>