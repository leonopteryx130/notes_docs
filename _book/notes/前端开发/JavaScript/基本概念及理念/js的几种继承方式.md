# js的几种继承方式

#### **1. 原型链继承**
具体方式：用父类的实例作为子类的原型。demo:
```
function People(name) {
    this.name = name
    this.sleep=function() {
        console.log("正在睡觉")
    }
}
function Woman(){}
Woman.prototype = new People('bob')
let womanObj = new Woman()
People.prototype.eat = function(food){
    console.log(this.name + '正在吃：' + food);
}
womanObj.eat("meat")
```
终端输出bob正在吃meat，原型链继承是最简单的继承实现方式，父类新增的属性和方法，子类都能正常访问。

**缺点**：
1. 但是原型链继承无法实现多继承，并且在创建子类实例的时候，无法向父类构造函数传入参数，也就是说womanObj创建的时候没有办法指定People构造函数里name 参数的值。
2. 如果有多个子类实例，其中一个子类修改了值，另一个子类的值也会改变。

代码demo:
```
function Parent() {
    this.name = "parent"
    this.data = [1, 2, 3]
}
Parent.prototype.getName = function() {
    console.log(this.name)
}
function Child() {
    this.type = "child"
}
Child.prototype = new Parent()

var children1 = new Child()
var children2 = new Child()

children1.data.push(4)
console.log('children1:', children1.data)
console.log('children2:', children2.data)
```

#### **2. 使用call，apply等api函数实现继承（也叫经典继承法）**
具体方式：复制父类的实例属性到子类。demo：
```
function People(name) {
    this.name = name
    this.sleep=function() {
        console.log("正在睡觉")
    }
}
function Woman(){
    this.age = 22
}
Woman.prototype = new People('bob')
function Famouswoman(){
    Woman.call(this)
}
let womanObj = new Famouswoman()
console.log(womanObj.name) //输出undefined
console.log(womanObj.age) //输出22
```
这样继承可以实现多继承（多次调用call和apply继承多个父类）

**缺点**：不能继承原型链上的属性和方法，只能继承父类的属性和方法，demo代码中name属性不是父类（Woman）的属性，是原型链（People）上的属性

#### **3. 组合式继承**
组合式继承相当于是将原型链继承和call/apply方法的api继承结合起来，也就是说要构造两次父类的构造函数，增大了开销，但是这种继承可以实现非常多的特性，比如可以继承原型链上的方法，可以实现多继承等，demo：
```
function Parent() {
    this.name = "parent"
    this.data = [1, 2, 3]
}
Parent.prototype.getName = function() {
    console.log(this.name)
}
//这块类似于经典继承法（使用call方法）
function Child() {
    Parent.call(this)
    this.type = "child"
}
//这块类似于原型链继承法
Child.prototype = new Parent()
//这块是组合继承的核心，让子类的构造器指向自己
Child.prototype.constructor = Child
var children1 = new Child()
var children2 = new Child()
children1.data.push(4)
//可以输出互不影响
console.log('children1:', children1.data)
console.log('children2:', children2.data)
```
**缺点**：构造函数执行了两次，增大了代码开销

#### **4. 原型式继承**
原型式继承使用到了ES5 里面的 Object.create 方法，这个方法接收两个参数：一是用作新对象原型的对象、二是为新对象定义额外属性的对象（可选参数）。 Object.create 方法默认是浅拷贝，因此不用担心赋值后，子类之间属性值相互影响的问题，但是由于是浅拷贝，要注意如果属性值是对象数组这类，值依然会相互影响，demo：
```
var Person = {
    name: "parent",
    friends: ["p1", "p2", "p3"]
}
let person1 = Object.create(Person)
person1.name = "tom"
person1.friends.push("jerry")
let person2 = Object.create(Person)
person2.friends.push("lucy")
console.log(person1.friends)
console.log(person2.friends)
```

#### **5. 寄生式继承**
使用原型式继承可以获得一份目标对象的浅拷贝，然后利用这个浅拷贝的能力再进行增强，添加一些方法，这样的继承方式就叫作寄生式继承。其优缺点与原型式继承并没有多少区别，可以理解为到组合寄生式继承的过渡，代码如下：
```
let parent = {
    name: "parent",
    friends: ["p1", "p2", "p3"],
    getName: function() {
        return this.name;
    }
}
function clone(original){
    let clone = Object.create(original)
    clone.getFriends = function(){
        return this.friends;
    }
    return clone;
}
let person = clone(parent)
console.log(person.getName())
console.log(person.getFriends())
```
#### **6. 组合寄生式继承**
组合寄生式继承其实相当于是在组合式继承的基础上，提供继承原型链的能力，这一点通过寄生式继承的浅拷贝可以实现，并且不需要构造两次构造函数。核心组件代码：
```
function inheritPrototype(subType, superType) {
    let prototype = Object(superType.prototype); 
    prototype.constructor = subType; // 增强对象
    subType.prototype = prototype; // 赋值对象
}
```
代码demo：
```
function SuperType(name) {
    this.name = name;
    this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function () {
    console.log(this.name);
};
function SubType(name, age) {
    SuperType.call(this, name);
    this.age = age;
}
inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function () {
    console.log(this.age);
};
```
#### **7. Class关键字继承**
最推荐的一种继承方式