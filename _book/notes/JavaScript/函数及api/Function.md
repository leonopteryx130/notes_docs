# Function

- **Function.displayname**:

这个方法用于获取或者设置一个方法的显示名称，但是如果没有提前设置显示名称，那么显示名称则永远为undefined，demo:

```
const myFunc = function(a) {
    console.log(a)
}
console.log(myFunc.displayName) //undifined
```

```
const myFunc = function(a) {
    console.log(a)
}
myFunc.displayName = 'yourFunc'
console.log(myFunc.displayName) //yourFunc
```

```
const myFunc = function(a) {
    console.log(a)
}
myFunc.displayName = 'yourFunc'
console.log(myFunc) //[Function: myFunc] { displayName: 'yourFunc' }
```

- **Function.name**:

这个方法用于获取函数名，这个方法不会被displayName影响，demo:

```
const myFunc = function(a) {
    console.log(a)
}
myFunc.displayName = 'yourFunc'
console.log(myFunc.name) //myFunc
```