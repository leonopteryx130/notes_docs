# webpack常用插件

### html-webpack-plugin

作用：当使用 webpack打包时，创建一个 html 文件，并把 webpack 打包后的静态文件自动插入到这个 html 文件当中。
	
一个最简单的demo:

```
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
  entry: 'index.js',
  output: {
    path: __dirname + '/dist',
    filename: 'index_bundle.js'
  },
  plugins: [
    new HtmlWebpackPlugin()
  ]
}
```

html-webpack-plugin 默认将会在 output.path 的目录下创建一个 index.html 文件， 并在这个文件中插入一个 script 标签，标签的 src 为 output.filename。
	
生成的代码如下：

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Webpack App</title>
  </head>
  <body>
    <script src="bundle.js"></script>
  </body>
</html>
```

### clean-webpack-plugin

作用：在每次打包时候清空output定义的输出文件夹。

用法：
非常简单，只需要安装，引用，注册插件即可。

安装：npm install clean-webpack-plugin

写法：

```
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const config = {
    ...
    plugins:[
        new CleanWebpackPlugin(),
    ],
}
```