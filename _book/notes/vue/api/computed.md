# computed

**用法**： computed中有两个重要的属性变量，计算属性变量和监听属性变量。计算属性变量在computed中定义，监听属性变量在data中定义，计算属性的值就是computed中方法的返回值（返回值由监听属性计算得来）。举个例子：
```
data() {
    return {
        formSum: {
            sum: 0,
            sumbutton: 100,
        },
    };
},
computed: {
    theSum() {
    // 计算数值的总和
        return this.formSum.sum + this.formSum.sumbutton
    },
},
```

案例中theSum是计算属性变量，formSum对象是监听属性变量。其中，计算属性的值会被缓存，只有当监听属性发生变化时候，计算属性才会重新计算，反之则使用缓存中的值。

**一些特性**：computed中，计算属性所对应的方法如果包含异步函数，那么方法不生效，即不会执行。如果方法中包含异步操作，还希望实现相同的效果，可以考虑在watch中去实现。

**getter和setter**：在上边的案例中，其实是默认使用了computed的getter方法，即根据依赖属性计算出计算属性的值，并读取。computed还有一个setter属性，当methods中的方法改变计算属性的值的时候，就会调用这个方法，通常会在这个方法中去改变依赖属性的值

**为什么要用computed?**

computed里的方法写入methods也是同样可以运行的，但是如果在methods写入方法，多次调用时候，无论依赖项的值是否改变，都要重新计算一次值，写入computed后，如果依赖项没有改变，会从缓存中读值，不必再进行过多的计算，提升了性能。