# webpack-chain

webpack的本质是一个JavaScript对象。webpack-chain的设计思路是通过链式调用的方法来配置webpack，webpack-chain的配置和普通的webpack是一样的，只是语法和表现方法不同。

#### **使用方法**：

如果希望直接在webpack.config.js文件中使用，是需要安装一下这个插件的：
```
npm install --save-dev webpack-chain
```
注：如果是 vue-cli-service 这类脚手架，会默认自动安装这个插件，可以直接使用。

在webpack.config.js中引入：

```
// webpack-chain 导出的是一个构造函数
const Config = require('webpack-chain')
// 创建 config 实例
const config = new Config()
//config链式配置代码
config.xxx()
        ...
// 调用 config.toConfig() 就会生成最终的 webpack 配置对象，直接导出即可
module.exports = config.toConfig()
```
注：如果是vue-cli-service脚手架，不需要额外引入，直接在chainWebpack中写即可。

webpack-chain有两种类型的API，ChainedMap类型和ChainedSet类型，直观区别是，ChainedMap类型没有传入参数，ChainedSet类型有传入参数。

#### **用法举例**：

配置entry和output:

```
config
  // 配置根选项 entry
  .entry('index')
  .add('src/index.js')
  .end()
  // 配置根选项 output
  .output
  .path('dist')
  .filename('[name].bundle.js')
```

等价于：

```
entry: { index: ['src/index.js'] },
output: { path: 'dist', filename: '[name].bundle.js' },
```

配置插件规则：

```
config.module
    // 创建具名的 rule 配置，方便之后可以访问并修改这个 rule
    .rule('lint')
    .test(/\.js$/)
    .pre()
    .include
    .add('src')
    .end()
    // 也可以创建具名的 use
    .use('eslint')
    .loader('eslint-loader')
    .options({
        rules: {
        semi: 'off',
    },
})
config.module
    .rule('compile')
    .test(/\.js$/)
    .include
    .add('src')
    .add('test')
    .end()
    .use('babel')
    .loader('babel-loader')
    .options({
    presets: [
        ['@babel/preset-env', { modules: false }],
    ],
})
```
等价于：

```
module: {
    rules: [
        {
        test: /\.js$/,
        enforce: 'pre',
        include: ['src'],
        use: [
            {
            loader: 'eslint-loader',
            options: { rules: { semi: 'off' } },
            },
        ],
        },
        {
        test: /\.js$/,
        include: ['src', 'test'],
        use: [
            {
            loader: 'babel-loader',
            options: {
                presets: [['@babel/preset-env', { modules: false }]],
            },
            },
        ],
        },
    ],
},
```