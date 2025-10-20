# vue数据劫持

### 定义

数据劫持，指的是在访问或者修改对象的某个属性时，通过一段代码拦截这个行为，进行额外的操作或者修改返回结果。

#### vue使用了两种方案实现了数据劫持：

- Object.defineProperty （vue2使用这种方法，demo代码见[用defineProperty方法实现数据劫持](../../JavaScript/code_demo/用defineProperty方法实现数据劫持.md)）
- Proxy （vue3使用这种方法，reactive()方法）

```Object.defineProperty``` 在处理数组的方法（比如pop，push等）的时候并不是很方便，所以vue3使用了es6的Proxy api，Proxy劫持的是整个对象，包括对象的属性的增加和删除Proxy都能检测到，但是Proxy依然监测不到嵌套对象的改变，因此vue3的源码中依然使用了递归的方式去遍历对象。