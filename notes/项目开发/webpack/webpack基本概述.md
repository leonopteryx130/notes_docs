# webpack基础概述

### **安装**：

如果希望在命令行或者package.json中使用webpack，那么必须安装webpack-cli工具包，webpack-cli是webpack的一个命令行工具。
```
npm i webpack webpack-cli -D
```

### **配置文件**:

webpack以及webpack相关命令（比如webpack-dev-server）运行的时候，会自动在根目录下读取webpack.config.js文件作为配置文件，配置文件的结构为：

```
module.exports = {
    
}
```

webpack命令也可以指定配置文件，通过--config或者-c参数指定配置文件，举例：

```
"scripts": {
  "build": "webpack --config ./build/webpack.dev.js",
  "dev": "webpack --config ./build/webpack.prod.js",
}
```
一般来说，打包和开发环境的webpack配置差异比较大需要特性化定制的时候需要这么写

### **入口文件和出口文件**:

webpack打包的默认入口文件是./src/index.js，出口文件的路径是./dist/main.js在项目根目录下新建src文件，并创建index.js文件，写入下边代码：

```
const a = 333
console.log(333)
console.log(a)
```

项目根目录运行npx webpack，会看到根目录生成一个dist文件夹，包含一个main.js文件。

<div align="left">
    <img src=./webpack基本概述1.png width=50% />
</div>

可以看到webpack把变量进行赋值，然后消去了多余的换行，这里压缩是因为webpack默认是生产模式。

webpack打包的入口文件和出口文件的路径可以进行一些配置，webpack.config.js配置文件中的entry和output属性对应就是入口文件和出口文件的配置...

### **开发模式和生产模式**：

- 开发模式：仅编译JS中的ES Module语法
- 生产模式：不仅能编译JS中的ES Module语法，还能压缩JS代码。压缩代码可以让代码体积更小，运行速度更快。

可以通过--mode参数来配置开发模式或者生产模式，命令行：

```
"scripts": {
  "build": "webpack --mode production",
  "dev": "webpack --mode development"
}
```
这个参数同样可以写入webpack.config.js文件里的mode属性，比如：

```
module.exports = {
    entry: "./app.js",
    mode: "development"
}
```

如果希望参数配置写入配置文件，并且可以通过命令行区分开发模式和生产模式，那么可以通过使用 不同的配置文件，在命令中修改配置文件路径的方法（参考本文档中配置文件一节）。常用的目录结构为:

<div align="left">
    <img src=./webpack基本概述2.png width=30% />
</div>

**tips**：webpack会自动根据mode参数的值设定process.env.NODE_ENV，也就是说这个环境变量不用单独赋值，可以直接在js代码中使用，它的值和mode参数的值一样。

### **loader和plugin**：

Webpack 就像一条生产线，要经过一系列处理流程后才能将源文件转换成输出结果（loader）。这条生产线上的每个处理流程的职责都是单一的，多个流程之间有存在依赖关系，只有完成当前处理后才能交给下一个流程去处理。而插件（plugin）就像是一个插入到生产线中的一个功能，在特定的时机对生产线上的资源做处理。

- **Loader**：用于对模块源码的转换，loader描述了webpack如何处理非javascript模块，并且在build中引入这些依赖。loader可以将文件从不同的语言（如TypeScript）转换为JavaScript。

- **Plugin**：目的在于解决loader无法实现的其他事，从打包优化和压缩，到重新定义环境变量，功能强大到可以用来处理各种各样的任务。
简而言之，loader可以理解成webpack的横向广度，有了loader，webpack才可以打包处理各种的扩展语言。而plugin可以理解为webpack的纵向深度，在生命周期内注入不同的插件来扩展更多的能力。