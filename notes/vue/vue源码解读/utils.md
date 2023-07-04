- #### **def**

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