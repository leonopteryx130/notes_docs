# perf.ts——vue组件性能标记

### 概述：

在开发环境下，vue2会标记每个组件初始化所消耗的时间，不过这个标记在源码里并没有被使用到，标记的实现依赖于performance API的mark和measure方法，具体代码文件在：```src/core/instance/init.ts```

### 源码解读：

源码如下（删去不必要部分）：

```
export let mark
export let measure

if (__DEV__) {
  const perf = inBrowser && window.performance
  if (.../*if主要用于校验performance，防止方法不存在报错*/) {
    mark = tag => perf.mark(tag)
    measure = (name, startTag, endTag) => {
      perf.measure(name, startTag, endTag)
      perf.clearMarks(startTag)
    }
  }
}
```

performance里的mark和measure

代码里的inBrowser的定义在文件：src/core/util/env.ts第五行：

```
export const inBrowser = typeof window !== 'undefined'
```
是一个boolean值，用于判断宿主环境（node还是浏览器）

### 使用：

mark和measure使用的地方不多，仅仅标记了vue组件的初始化时间和渲染时间，分别在```src/core/instance/init.ts```和```src/core/instance/lifecycle.ts```两个文件中用到。

整个vue项目代码中仅仅对初始化和渲染时间做了标记，但是并没有在任何地方读取这个标记（没有调用```performance.getEntriesByName```方法）
