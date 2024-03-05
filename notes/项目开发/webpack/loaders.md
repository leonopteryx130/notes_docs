# loaders

### loader优先级

可以通过enforce属性指定loader的优先级。4 类 loader 的执行优级为：```pre > normal > inline > post``` 。如果优先级一样，那么执行顺序为从后向前执行。

```
module: {
  rules: [
    {
      test: /\.js$/,
      loader: "loader1",
    },
    {
      test: /\.js$/,
      loader: "loader2",
    },
    {
      test: /\.js$/,
      loader: "loader3",
    },
  ],
}
```

此时loader执行顺序：loader3 - loader2 - loader1

```
module: {
  rules: [
    {
      enforce: "pre",
      test: /\.js$/,
      loader: "loader1",
    },
    {
      // 没有enforce就是normal
      test: /\.js$/,
      loader: "loader2",
    },
    {
      enforce: "post",
      test: /\.js$/,
      loader: "loader3",
    },
  ],
}
```
此时loader执行顺序：loader1 - loader2 - loader3

### 使用内联方法定义loader

绝大多数情况下，loader都是声明在module.rules里，但是如果某个js文件引入资源所需要的loader与其他js文件有差异，这种情况就需要用到内联模式的loader。内联loader通常写在业务的js代码里，比如app.js需要引入同文件夹下的app.css文件，需要用到style-loader和css-loader，写法如下：

```
import "style-loader!css-loader!./app.css"
```

比如需要使用raw-loader引入data.txt文件并存为字符串

```
import data from "raw-loader!./data.txt"
```

**内联loader注意事项**：

- 内联方式会覆盖配置中的所有普通 loader（normal loader）。

- 使用 ! 前缀可以禁用所有已配置的普通 loader。

- 使用 !! 前缀可以禁用所有已配置的 loader（包括 normal、pre-loader 和 post-loader）。

- 内联方式的 loader 执行顺序从右到左，即最后一个 ! 后面的 loader 最先执行。

**举个例子**：

```
import styles from "!loader1!loader2?options!./file.ext"
```
- 前缀！表示禁用webpack中已经配置的普通loader
- 其余的!: 表示 loader 分隔符。
- loader1: 第一个要使用的 loader 名称。
- loader2: 第二个要使用的 loader 名称，以此类推。
- ?options: 用于传递选项给 loader，可包含查询参数。
- ./file.ext: 要处理的文件路径。

### 常用的loader

- babel-loader

babel-loader 用于将 ES6 代码转换为 ES5 代码，以便浏览器能够识别。

- css-loader

css-loader 用于解析 import 进来的 CSS 文件，并将其转换为 CommonJS 模块，然后交给后续的 loader 处理。

- style-loader

style-loader 用于将 CSS 代码注入到 DOM 中，使 CSS 代码生效。

- sass-loader

sass-loader 用于将 Sass 代码转换为 CSS 代码，然后交给后续的 loader 处理。

- html-loader

html-loader 用于处理 HTML 文件，将 HTML 文件转换为 JS 字符串。

- raw-loader

raw-loader 用于将文件转换为 JS 字符串。

- eslint-loader

eslint-loader 用于检查 JS 代码是否符合规范。

- vue-loader

vue-loader 用于将 Vue 单文件组件转换为 JS 代码。

- babel-plugin-import

babel-plugin-import 用于按需加载组件，可以替代 vue-loader。