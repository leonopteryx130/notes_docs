### **def**

**文件路径**：/src/core/util/lang.ts

**源代码**：

```
export function def(obj: Object, key: string, val: any, enumerable?: boolean) {
    Object.defineProperty(obj, key, {
    value: val,
    enumerable: !!enumerable,
    writable: true,
    configurable: true
    })
}
```

**源码解读**：这个代码相当于对Object.defineProperty进行了一个封装

**参数**：三个必传参数obj，key和val。一个可选参数enumerable。

**作用**：给obj对象添加一组属性key-val对，并设置属性为可写，可配置，可遍历性根据enumerable确定

****

### **makeMap**

**文件路径**：/src/shared/util.ts

**源代码**：

```
export function makeMap(
    str: string,
    expectsLowerCase?: boolean
): (key: string) => true | undefined {
    const map = Object.create(null)
    const list: Array<string> = str.split(',')
    for (let i = 0; i < list.length; i++) {
    map[list[i]] = true
    }
    return expectsLowerCase ? val => map[val.toLowerCase()] : val => map[val]
}
```

**源码解读**：方法有一个必传参数str和一个可选参数expectsLower。str是一个字符串格式的参数，这个字符串在源码里会以逗号为节点，分割为一个数组，举个例子：```car,bus,bike,car``` 这段字符串会被分割为 ```[car, bus, bike, car]```这样一个数组，这个数组会转化为map结构：
```
{
    car: true,
    bus: true,
    bike: true
}
```
该方法返回值是一个方法，这个方法支持输入一个字符串，判断字符串是否在上边map结构的key里。

该方法的第二个参数```expectsLower```是一个布尔值，如果设置为true，那么返回值方法传入的字符串会被转化为全小写再判断

**用法**：

```
const isAnimalinZoo = makeMap('cat,dog,monkey', true)

isAnimalinZoo('Dog') // true
isAnimalinZoo('Tiger') // false
```