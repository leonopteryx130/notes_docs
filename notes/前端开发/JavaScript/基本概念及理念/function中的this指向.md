# function中的this指向

js定义，当function没有明确调用者时，this指向全局对象，这里的明确调用者指的是类的实例对象或者对象

先看一个简单的例子：

```
var obj = {
 name: 'coco',
 display: function(){
   console.log(this.name); // this 指向 obj
  }
};
obj.display(); // coco
```
这段代码很容易理解，因为函数有明确的调用者 obj，所以this指向obj

那如果定义一个方法并直接执行呢？
```
function display() {
    let name = 'coco'
    console.log(this.name);
}
display() //undefined
```

因为display没有明确的调用者，所以指向全局，如果把name挂载到全局，那么就可以输出：


```
globalThis.name = 'b'
function display() {
    console.log(this.name);
}
display() // b
```
如果使用new的方式实例化并且在this上绑定name，那么调用者就是实例对象，this也指向这个实例对象
```
function display() {
    this.name = 'coco'
    console.log(this.name);
}
new display() // coco
```
在使用一些设计模式或者回调函数的时候，就很容易出现this指向错误的问题：

```
var obj = {
    name: 'coco',
    display: function(){
        console.log(this.name); // this 指向 obj
    }
};
function handleClick(callback) {
    callback()
}
handleClick(obj.display);
```
这段代码相当于把display对应的匿名函数输入handleClick当作参数，在函数体内直接执行，等价于：

```
var obj = {
    name: 'coco',
    display: function(){
        console.log(this.name); // this 指向 obj
    }
};
function handleClick() {
    console.log(this.name); // this 指向 obj
}
handleClick();
```
因此this指向global，输出undefined

再举个例子：

```
class Foo {
    constructor(name){
        this.name = name
    }
    display(){
        console.log(this.name);
    }
}
var foo = new Foo('coco');
foo.display(); // coco
// 下面例子类似于在 React Component 中 handle 方法当作为回调函数传参
var display = foo.display;
display() // TypeError: this is undefined
```
如果希望display方法始终指向某个对象，可以使用bind对this进行绑定:

```
var obj = {
    name: 'coco',
    display: function(){
        console.log(this.name); // this 指向 obj
    }
};
obj.display = obj.display.bind(obj); // 改变 this 指向
var func = obj.display;
func() // coco
```
在类里，如果想把某个类方法导出到变量里，便于灵活调用，this指向会丢失

```
class Cls {
    constructor() {
        this.name = "John";
    }
    
    display() {
        console.log(this.name);
    }
}
    
var myCls = new Cls();
var display = myCls.display;
display(); 
```
因为display方法没有明确的调用者，所以指向global
这个时候需要在constructor里绑定一下：

```
class Cls {
    constructor() {
        this.name = "John";
        this.display = this.display.bind(this); // 绑定 this
    }
    
    display() {
        console.log(this.name);
    }
}
    
var myCls = new Cls();
var display = myCls.display;
display(); // 输出John
```
React class组件在jsx里的事件函数需要bind在constructor里也是这个原因。
日常Class实现这个目的也可以在class外操作:

```
class Cls {
    constructor() {
        this.name = "John";
    }
    
    display() {
        console.log(this.name);
    }
}
    
var myCls = new Cls();
myCls.display = myCls.display.bind(myCls);
var display = myCls.display;
display(); 
```
这样也是可行的