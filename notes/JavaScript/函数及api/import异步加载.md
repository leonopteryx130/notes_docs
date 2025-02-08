# import()异步加载

ES6中import关键字总是会被javascript静态解析，优先于其他模块执行，因此无法实现按需加载和懒加载（也就是动态加载，即代码片段运行时加载），在有些时候这种操作会增大性能消耗。

为了实现动态加载，ES6引入了import()函数，是一种类似于node中require的函数，与之不同的是，require是同步加载，import是异步加载。

import()返回一个Promise对象， 因此可以调用then方法，then方法接受一个函数作为参数，函数又接受一个对象作为参数，对象有个default属性，其属性值就是import参数对应js文件的默认导出值（即export default），如果js文件有多个导出值，那么对象也就会有多个属性值。举个例子：
首先准备一个js文件，暴露两个可调用的方法：
```javascript
//method.js
export default function add(a, b) {
    return a+b
}
export function minus(a, b) {
    return a-b
}
```
在main.js里边使用import()导入method.js文件：
```javascript
//main.js 
import("./src/method.js").then((a) => {
    console.log(a)
})
```
输出：
<div align="left">
    <img src=./import异步加载.png width=30% />
</div>
a的值是一个对象，可以对通过a的属性来调用函数：

```javascript
import("./src/method.js").then((a) => {
    console.log(a.minus(9, 8))
    console.log(a.default(9, 8))
})

//分别输出1 和 17
```

由于default是js里边一个关键字，因此不能通过解构赋值的方法获取到export default对应的方法，但是其他方法可以通过解构赋值获取到。

```javascript
import("./src/method.js").then(({minus}) => {
    console.log(minus(9, 8))
})

//输出1
```