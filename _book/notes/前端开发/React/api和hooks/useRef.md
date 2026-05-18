# useRef

## 作用

useRef 返回一个可变的 ```ref``` 对象，其 ```.current``` 属性被初始化为传入的参数（```initialValue```）。```.current```属性可以被修改，并且修改```.current```属性不会触发组件重新渲染。

## 示例

```javascript
const intervalRef = useRef(0);
```

## 注意

1. ```useRef``` 返回的对象在组件的整个生命周期内保持不变，即使组件重新渲染也不会改变。
3. ```useRef``` 返回的对象通常用于存储不需要触发组件重新渲染的值，例如定时器的 ID、DOM 元素的引用等。