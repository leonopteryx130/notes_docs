# react里的css module

### css module是什么？

CSS Modules 既不是官方标准，也不是浏览器的特性，而是在构建步骤（例如使用 Webpack 或 Browserify）中对 CSS  class名和选择器限定作用域的工具

### 为什么要使用css module？

CSS的规则都是全局的，任何一个组件的样式规则，都对整个页面有效。产生局部作用域的唯一方法，就是使用一个独一无二的class的名字，不会与其他选择器重名。但是多人协同开发一个大型项目的时候，很难保证class名独一无二，因此需要在开发的时候给css设定一个作用域的概念。css module的目的就是解决这个问题

### css module是如何解决的？

css module就是通过在编译阶段，给class名称后边加一个hash值，来确保class名的独一无二，在react中，css文件和jsx之间，可以通过import引入的方式，来确保元素class和样式的映射关系。

### 基本用法：

组件app.js
```
import React from 'react';
import style from './App.css';
export default () => {
  return (
    <h1 className={style.title}>
      Hello World
    </h1>
  );
};
```
App.css
```
.title {
    color: red;
}
```
打包后，构建工具会将类名style.title编译成一个哈希字符串，hash长度，字符串拼接可以在webpack里配置，打包后结果：

```
<h1 class="_3zyde4l1yATCOkgn-DBWEL">
  Hello World
</h1>
```
css也会被编译
```
._3zyde4l1yATCOkgn-DBWEL {
    color: red;
}
```
css module也支持全局作用域，在css文件中，使用 :global(.xxx)声明的样式，不会被编译为hash（这里xxx是class名称），写法：

```
.title {
    color: red;
}
:global(.title) {
    color: green;
}
```
当项目中用到一些组件库，不方便修改样式时候，可以通过这种方式实现穿透

webpack配置：

```
test: /\.(s[ac]ss|css)$/i,
use: [
    // 将 JS 字符串生成为 style 节点s
    'style-loader',
    // 将 CSS 转化成 CommonJS 模块
    {
        loader: 'css-loader',
        // 启动css module并配置
        options: {
            importLoaders: 1,
            modules: {
                auto: /\.(s[ac]ss|css)$/i,
                localIdentName: '[local]_[hash:base64:10]', //在css后边添加10位的hash
            },
        },
    },
    // 将 Sass 编译成 CSS
    'sass-loader',
]
```