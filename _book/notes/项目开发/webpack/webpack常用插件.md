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

### webpack-merge
作用背景：

项目开发通常分为生产环境和开发环境，两个环境会包含一些共用的基础配置，但是两个环境的侧重点往往会不一样。在开发环境中，我们需要：强大的 source map 和一个有着 live reloading(实时重新加载) 或 hot module replacement(热模块替换) 能力的 localhost server。而生产环境目标则转移至其他方面，关注点在于压缩 bundle、更轻量的 source map、资源优化等，通过这些优化方式改善加载时间。

实际开发中通常会把webpack配置文件分成开发环境和生产环境，但是开发中如果需要修改一些共有配置，可能就要修改两处代码，对项目开发而言并不是很友好，因此可以将共有配置单独写一个文件，搭配webpack-merge实现共有配置和特性化配置的融合。

配置文件目录通常为如下结构：

<div align="left">
    <img src=./webpack常用插件.png width=30% />
</div>
在dev或者prod配置文件中可以用webpack-merge合并common里边的配置，举个例子：

```
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');
module.exports = merge(common, {
    mode: 'development',
    devtool: 'inline-source-map',
    devServer: {
        static: './dist',
    },
});
```