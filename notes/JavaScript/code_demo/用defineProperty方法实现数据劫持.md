# 用defineProperty方法实现数据劫持

### 数据劫持基本定义：

见[vue数据劫持](../../vue/基本概念/vue数据劫持.md)

### 目标：

输入一个对象，对指定的key进行数据劫持，实现在读取和修改该数据的时候做一些操作。

### 参数

 - obj：需要劫持的对象
 - key：需要劫持的key
 - val：需要劫持的key初始化的值（这个也可以不设定）

代码如下：

```javascript
const dataHijacking = (obj, key, val) => {
  const property = Object.getOwnPropertyDescriptor(obj, key)
  // 因为这里只是添加一些拦截，并不是要完全重写getter和setter，所以这里需要先获取原来的getter和setter，然后在新的getter和setter中调用原来的getter和setter
  const getter = property && property.get 
  const setter = property && property.set 

  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function() {
      const value = getter ? getter.call(obj) : val // 如果有getter，就调用getter，否则直接返回值
      console.log(`getting ${key}: ${value}`) // 这里也可以是在读取值的时候做一些操作
      return value
    },
    set: function(newValue) {
      const value = getter ? getter.call(obj) : val
      if (newValue === value) { return } // 如果新值和旧值相同，直接返回
      setter ? setter.call(obj, newValue) : val = newValue // 如果有setter，就调用setter，否则直接赋值
      console.log(`setting ${key}: ${newValue}`) // 这里也可以是在监听的值改变
    }
  })
}
```

尝试定义一个对象，并指定一个数据劫持的key，看看结果

```javascript
const testObj = {
    name: 'bob',
    score: {
        math: 90,
        english: 80
    }
}

dataHijacking(testObj, 'name', testObj.name)
testObj.name = 'tom'
console.log(testObj.name)
```

输出：

```
setting name: tom
getting name: tom
tom
```

但是如果是嵌套的属性，比如上面的score，那么就需要递归调用dataHijacking方法，对score进行数据劫持，在对score.math重新赋值的时候，setter里的代码就不会生效。比如：

```javascript
testObj.score.math = 100
```

因此在setter里需要递归遍历属性对象，对属性对象里的每一个key进行数据劫持。

```javascript
const observe = (obj) => {
    if (typeof obj !== "object" || obj == null) {
        return
    }
    if (Array.isArray(obj)) {
        // 如果是一个数组要重写数组上原型上的方法 
        Object.setPrototypeOf(obj, proto)
        for (let i = 0; i < obj.length; i++) {
            let item = obj[i];
            observer(item)
        }
    } else {
        for (let key in obj) {
            dataHijacking(obj, key, obj[key])
        }
    }
}

const dataHijacking = (obj, key, val) => {
    observe(val)
    Object.defineProperty(obj, key, {
        enumerable:true,
        configurable:true,
        get: function() {
            console.log(`getting ${key}: ${val}`)
            return val
        },
        set: function(newValue) {
            if (newValue === val) { return }
            console.log(`setting ${key}: ${newValue}`)
            observe(newValue)
            val = newValue
        }
    })
}
```

现在修改testObj.score.math的值，setter里的代码就会生效了。

```javascript
dataHijacking(testObj, 'score', testObj.score)
testObj.score.math = 100
```

输出：

```
getting score: [object Object]
setting math: 100
```