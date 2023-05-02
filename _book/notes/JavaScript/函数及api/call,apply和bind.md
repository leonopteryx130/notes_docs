# call，apply和bind

ECMAScript 规范给所有函数都定义了 call 与 apply 两个方法，它们的应用非常广泛，它们的作用也是一模一样，只是传参的形式有区别而已。js里call()方法可以改变this的指向，call 方法第一个参数也是作为函数上下文的对象，但是后面传入的是一个参数列表，而不是单个数组。写法规范：
```
obj1(or function).call(obj2,argument1,argument2)
```
call的作用就是把obj1的方法放到obj2上执行，后边的argument被作为传入参数。这样obj1可以继承obj2的一些属性直接执行，demo coding:
```
function add (x) { 
    console.log (x + this.num);
} 
function minus () { 
    this.num = 3
} 
add.call (new minus(), 5);    
//终端输出 8
```
可以看到，demo里边打的call函数将add方法放在了minus的实例对象里边执行，这样add函数可以共享minus中this.num属性，因此即使不在add方法里声明this.num，也可以正确获取到对应的值。

**apply 方法**传入两个参数：一个是作为函数上下文的对象，另外一个是作为函数参数所组成的数组，code:
```
var obj = {
    name : 'linxin'
}
function func(firstName, lastName){
    console.log(firstName + ' ' + this.name + ' ' + lastName);
}
func.apply(obj, ['A', 'B']);    // A linxin B
```
可以看到，obj 是作为函数上下文的对象，函数 func 中 this 指向了 obj 这个对象。参数 A 和 B 是放在数组中传入 func 函数，分别对应 func 参数的列表元素。

对于什么时候该用什么方法，其实不用纠结。如果你的参数本来就存在一个数组中，那自然就用 apply，如果参数比较散乱相互之间没什么关联，就用 call。

### **利用call或者apply实现继承**
demo coding:
```
function f(){    
    this.a ="a";    
    this.b = function(){    
        alert("b");
    }
}
function e(){    
    f.call(this);     
}
var c = new e();
alert(c.a);  //弹出a
c.b();    //弹出b
```
这段代码将函数f放在了函数e里边执行，因此实例化函数e，也可以得到函数f的属性和方法。

bind方法和call方法很相似，区别在于，call方法直接会执行对应的函数，而bind方法会返回一个function，相当于把函数绑定到某个对象上产生一个新的方法，支持直接调用，这个新的方法是被改变了上下文的方法。用法：
```
var obj = {
    name: 'linxin'
}
function func() {
    console.log(this.name);
}
var func1 = func.bind(obj);
func1();
```
bind方法参数的使用：
```
function func(a, b, c) {
    console.log(a, b, c);
}
var func1 = func.bind(null,'linxin');
func('A', 'B', 'C');            // A B C
func1('A', 'B', 'C');           // linxin A B
func1('B', 'C');                // linxin B C
func.call(null, 'linxin');      // linxin undefined undefined
```
