# JSON.stringfy

### 作用：

将一个js对象或值转化为字符串

### 转换规则：

- 对象里属性之为undefined，该属性会被忽略

    ```
    let obj = {
        a: '1',
        b: undefined
    }
    console.log(JSON.stringify(obj))
    ```
    输出：
    ```
    {"a":"1"}
    ```

- 函数，symbol等类型的值，会被转换为undefined，如果是作为对象的属性值，转换为undefined后，该属性会被忽略

    ```
    let obj = {
        a: '1',
        b: function() {
            console.log(this.a)
        }
    }
    console.log(JSON.stringify(obj))
    console.log(JSON.stringify(function() {console.log("2")}))
    ```
    输出：

    ```
    {"a":"1"}
    undefined
    ```

- 如果是在数组中，undefined，symbol，function类型会转换为null
    ```
    let arr = ['aaa', undefined, Symbol(11), function() {console.log("1")}]
    console.log(JSON.stringify(arr))
    ```
    输出：

    ```
    ["aaa",null,null,null]
    ```
- Date对象会直接输出为时间字符串：

    ```
    let obj = {
        a: new Date()
    }
        console.log(JSON.stringify(obj))
    ```
    输出：

    ```
    {"a":"2023-12-23T19:11:10.373Z"}
    ```

### 踩坑记录：

Node.js环境下，使用console.log打印构造函数（或者类）的原型对象时候，会自动把结果转化为字符串，可以理解为自动调用了JSON.stringfy方法，看如下代码：

```
class Cat {
    constructor(name) {
        this.name = name;
    }
    run() {
        console.log(this.name + ` is runnning`);
    }
}
console.log(Cat.prototype)
```

在node.js环境下输出为一个空对象：```{}```

在浏览器console环境下，可以正常看到constructor方法和run方法：

<div align="center">
    <img src=./JSON.stringfy.png width=60% />
</div>