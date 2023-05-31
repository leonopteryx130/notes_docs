# webpack-dev-server

#### **webpack-dev-server的作用**：

webpack可以将项目打包成静态资源，通过服务器nginx等方法发布，但是在开发环境中，前端开发也希望项目可以在本地启动，并可以热更新，这个时候就需要用到webpack-dev-server。

#### **一个最小案例**：

首先安装webpack系列插件：

```
npm i webpack webpack-cli webpack-dev-server -D
```

在项目根目录下创建一个index.html文件，并写一些html代码，同时在根目录创建一个webpack.config.js文件：

```
const path = require('path');
module.exports = {
  devServer: {
    static: {
      directory: path.join(__dirname, ''),
    },
    compress: true,
    port: 9000,
  },
};
```
在package.json文件中写入命令：

```
"scripts": {
    "test": "webpack-dev-server"
}
```
在命令行运行npm run test，就可以在本地localhost:9000看到index.html所对应的页面了，也可以使用命令npx webpack-dev-server也是一样的。

**注**：因为webpack-dev-server是webpack的功能，因此webpack-cli提供了简单语法运行webpack-dev-server，命令webpack serve的作用和webpack-dev-server是一样的。