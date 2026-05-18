# template标签

H5中的template标签在页面中不会显示，并且在浏览器检查元素的时候也看不到这个标签，可以理解为一个模板占位符。常常在vue里搭配v-for使用。举个例子，如果想实现通过for循环来遍历生成dom元素，不用template实现：

```
<template>
    <div class="root">
        <div v-for="item,index in 5">
            <div>测试{{index}}</div>
        </div>
    </div>
</template>
```

渲染结果会多很多层div：

<div align="left">
    <img src=./template标签1.png width=40% />
</div>
可以看到有很多多余的div出现，如果用template实现：

```
<template>
    <div class="root">
        <template v-for="(item,index) in 5">
            <div>测试{{index}}</div>
        </template>
    </div>
</template>
```
<div align="left">
    <img src=./template标签2.png width=70% />
</div>
可以看到template标签不会渲染，因此不会出现过多的div元素。所以template相当于一个占位符的效果
