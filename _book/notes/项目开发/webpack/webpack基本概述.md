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