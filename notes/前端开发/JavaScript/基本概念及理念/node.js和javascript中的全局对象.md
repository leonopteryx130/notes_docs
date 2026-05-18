# node.js和javascript中的全局对象

javaScript 中有一个特殊的对象，称为全局对象（Global Object），这个对象不需要定义，会自动创建，它及其所有属性都可以在程序的任何地方访问，即全局变量。

在浏览器 JavaScript 中，通常 window 是全局对象， 而 Node.js 中的全局对象是 global，所有全局变量（除了 global 本身以外）都是 global 对象的属性。

在 Node.js 我们可以直接访问到 global 的属性，而不需要在应用中包含它。同样在浏览器调试界面或者html的script标签里边也是可以访问到window对象，以及可以直接访问到window对象的全部属性。甚至可以给全局对象中添加属性，变成全局属性

node.js中：
```
global.__DEV__ = 'develop'
console.log(global)
```
是可以输出一个对象，对象中包含__DEV__属性