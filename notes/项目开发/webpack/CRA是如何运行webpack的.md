# CRA是如何运行webpack配置文件的

创建一个CRA app后运行yarn eject可以反编译出webpack的配置文件，并且package.json中的script也会变，反编译后的package.json中：

```
  "scripts": {
    "start": "node scripts/start.js",
    "build": "node scripts/build.js",
    "test": "node scripts/test.js"
  },
```

可以看到，运行（start）和打包（build）命令被封装到不同的js代码里，在build.js文件的第22行和第50行，有如下代码：

```
//第22行
const configFactory = require('../config/webpack.config');

//第50行
const config = configFactory('production');
```

表示打包文件引入了webpack.config.js的配置文件，并输入参数production，这与配置文件（webpack.config.js）中90-92行布尔值相对应：

<div align="center">
    <img src=./CRA是如何运行webpack.png width=80% />
</div>