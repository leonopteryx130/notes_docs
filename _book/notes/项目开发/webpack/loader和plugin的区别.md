# loader和plugin的区别

### 作用区别：

- loader 是文件加载器，能够加载资源文件，并对这些文件进行一些处理，诸如编译、压缩等，最终一起打包到指定的文件中

- plugin 赋予了 webpack 各种灵活的功能，例如打包优化、资源管理、环境变量注入等，目的是解决 loader 无法实现的其他事

### 执行时机：

- loader 是在模块被引入时执行，在打包文件之前
- plugin 是在构建过程中的整个编译周期都起作用

### 使用：

loader通常在module.rules里边写，plugin通常在plugins里边写，可以通过enforce属性指定loader的优先级。4 类 loader 的执行优级为：```pre > normal > inline > post``` 。如果优先级一样，那么执行顺序为从后向前执行。

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