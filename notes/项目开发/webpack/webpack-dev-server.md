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

#### **webpack-dev-server常用命令参数**：

- open：是否自动开启浏览器，默认：false，当设置为true，开启dev-server服务的时候，会自动在浏览器打开项目。
- port ：端口号 默认：8080 可以单独设置服务器的端口号，如果默认的8080被占用的情况，这个配置就派的上用场啦。
- contentBase：定义开启服务器的文件夹，默认：当前项目根目录 一般来说不需要设置这个选项，除非你需要DevServer 来访问其他的静态文件夹。
- hot：该配置项是指模块替换功能，DevServer 默认行为是在发现源代码被更新后通过自动刷新整个页面来做到实时预览的，但是开启模块热替换功能后，它是通过在不刷新整个页面的情况下通过使用新模块替换旧模块来做到实时预览的。这么做的好处是，可以减少一些不必要的请求。

这些参数也都可以写在webpack.config.js文件中，例如：

```
devServer：{
    open:true,
    port:端口号,
    contentBase:'路径',
    hot:true
}
```