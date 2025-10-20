# ReactiveFlags标记

### 源码：

```typescript
/**
 * 代码位置
 * vue3/core/packages/reactivity/src/constants.ts
 */
export enum ReactiveFlags {
  SKIP = '__v_skip', //无需响应的对象
  IS_REACTIVE = '__v_isReactive', //是否是reactive响应式对象
  IS_READONLY = '__v_isReadonly', //是否是只读数据
  IS_SHALLOW = '__v_isShallow', 
  RAW = '__v_raw', // 取原始对象
  IS_REF = '__v_isRef', //是否是ref响应式对象
}
```

这里的```ReactiveFlags```枚举其实是Target对象属性的标志位（枚举标志位的定义见[枚举标志位](../../../项目开发/设计模式/枚举标志位.md)），这里的Target对象本质上就是vue的响应式对象，通常为```reactive()```或者```ref()```的返回值。

这些属性在业务中基本不会用得到，但是在源码中对于一些功能逻辑的判断来说，非常重要。在源码中也定义了一些方法可以对Target对象的属性进行简单的逻辑处理。

```typescript
/*
 * 代码位置
 * vue3/core/packages/reactivity/src/reactive.ts
 * 
 * 用于判断Target对象是否只读
 */
export function isReadonly(value: unknown): boolean {
  return !!(value && (value as Target)[ReactiveFlags.IS_READONLY])
}
```

```typescript
/*
 * 代码位置
 * vue3/core/packages/reactivity/src/reactive.ts
 * 
 * 用于判断Target对象是否是Reactive创建的响应式对象
 */
export function isProxy(value: any): boolean {
  return value ? !!value[ReactiveFlags.RAW] : false
}
```

```typescript
/*
 * 代码位置
 * vue3/core/packages/reactivity/src/reactive.ts
 * 
 * 用于判断Target对象是否是Reactive创建的对象
 */
export function isReactive(value: unknown): boolean {
  if (isReadonly(value)) {
    return isReactive((value as Target)[ReactiveFlags.RAW])
  }
  return !!(value && (value as Target)[ReactiveFlags.IS_REACTIVE])
}
```