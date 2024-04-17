# 设计模式——mixin模式

### 什么是mixin模式？

为了在一个类中能够继承另一个类的某些方法，以较低的复杂性达到复用的目的（只继承类里的部分方法，不用继承整个类，减小开销和复杂性）。

### 实现方法：

核心是需要写一个mixin方法，这个方法输入两个类和一个数组，两个类分别是需要继承其他方法的类和被继承的类，数组是被继承的类的方法名。

**首先定义两个类**：

```
class Cat {
    constructor(name) {
        this.name = name;
    }
    run() {
        console.log(this.name + ` is runnning`);
    }
}
class Bird {
    constructor(name) {
        this.name = name;
    }
    fly() {
        console.log(this.name + `is flying`);
    }
}
```

**然后定义mixin方法**：

```
const mixin = (receivingClass, givingClass, ...funcNames) => {
    for (let funcName of funcNames) {
        receivingClass.prototype[funcName] = givingClass.prototype[funcName];
    }   
}
```

**最后使用mixin方法**：

```
mixin(Cat, Bird, 'run', 'fly');
const cat = new Cat('cat');
cat.run();
cat.fly();
```

**输出结果**：

```
Cat Tom is runnning
Cat Tom is flying
```