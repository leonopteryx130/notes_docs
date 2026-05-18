# watch

**作用**：vue中watch和computed的作用类似，computed是监听依赖属性的改变从而执行代码更新计算属性的值。watch的用法更为通用一点，侦听属性作为方法名，如果侦听属性发生变化，那么执行对应的方法。

**基本用法**：
侦听属性的方法传入两个参数，第一个是newName，第二个是oldName，demo：

```
watch: {
    firstName(newName, oldName) {
        this.fullName = newName + ' ' + this.lastName;
    }
} 
```

方法默认是调用了handler，完整写法：

```
watch: {
    firstName: {
        handler(newName, oldName) {
            this.fullName = newName + ' ' + this.lastName;
        },
        immediate: true
    }
} 
```

immediate表示组件初次渲染后立即执行一次

如果只希望监听对象的某个属性，可以用字符串的形式来表示，写法如下：

```
watch: {
    "obj.name": {
        handler(newValue, oldValue) {
            console.log("obj 的 name 发生了变化");
            console.log('newValue: ' + newValue);
            console.log('oldValue: ' + oldValue);
        },
    },
},
```

**需要注意**：watch所监听的数据必须是data中声明的或者父组件传递过来的prop数据。

如果侦听属性是一个对象，默认情况下，handler只侦听这个属性本身引用的变化，但是变化往往存在于对象的某个具体属性上，如果希望捕捉到这些属性，需要设置deep属性为true。需要注意：watch所监听的数据必须是data中声明的或者父组件传递过来的prop数据。

如果侦听属性是一个对象，默认情况下，handler只侦听这个属性本身引用的变化，但是变化往往存在于对象的某个具体属性上，如果希望捕捉到这些属性，需要设置deep属性为true。

