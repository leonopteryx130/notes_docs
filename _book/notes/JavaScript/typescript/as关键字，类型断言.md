# as关键字，类型断言

通常情况下，开发者会比TypeScript更清楚某个值的详细类型，TypeScript在编译的时候会先检查这个值的类型，这个行为需要一点时间，如果使用断言，就相当于直接告诉TypeScript这个值的类型，不用检查，TypeScript会假设开发者已经做了必要的检查。这样提高了编译速度，但是一定是开发者非常确定某个值的类型时候才可以使用。

类型断言由两种语法形式：
- 尖括号语法：
```typeScript
let someValue:any = 'this is a string'；
let strLength:number = (<string>someValue).length；
```

- as语法
```typeScript
let someValue:any = 'this is a string'；
let strLength:number = (someValue as string).length；
```

两种形式是等价的。至于使用哪个大多数情况是凭个人爱好；然而，当你在```TypeScript```里使用```JSX```时，只有 ```as``` 语法断言是被允许的。


**Tips**：当设置类型为any时，一般都可以用断言。

### 使用断言必须满足以下条件之一

- 源类型和目标类型是兼容的，有父子关系的

```typeScript
const a: "123" = "123"
const b: string = a as string // ok
```

```typeScript
const a: string = "123"
const b: "123" = a as "123" // ok
```

```typeScript
const a: "123" = "123"
const b: "456" = a as "456" // error
```
因为`"123"`和`"456"`没有父子关系，所以断言失败。

- unknown 是 **安全的顶层类型**（所有类型的父类型），**可以接受任何类型的赋值**，但 不能直接操作其值（必须通过类型断言或类型收窄后才能使用）。

```typeScript
const a: unknown = "123"
const b: string = a as string // ok
```

```typeScript
const a: string = "123"
const b: unknown = a as unknown // ok
```

-  如果目标类型或者源类型是any，any既是顶层类型，又是底层类型（破坏类型系统），所以可以断言成任何类型。

```typeScript
const a: any = "123"
const b: string = a as string // ok
```

```typeScript
const a: string = "123"
const b: any = a as any // ok
```

- any 会 **完全禁用类型检查**，可能导致运行时错误。应尽量避免使用 as any，优先用 as unknown。

```typeScript
const a: any = "123"
const b: number = a as number // 编译通过，但运行时可能出错！
```

### 双重断言

刚才的例子中，使用any或者unknown都可以断言成任何类型，所以在一些不能断言成目标类型的情况下，可以使用双重断言，先断言成any或者unknown，再断言成目标类型。

```typeScript
const a: string = "123"
const b: number = a as unknown as number // ok
```

tips: 这种双重断言 完全绕过类型检查，如果 a 不是有效的 number，会导致运行时错误（如 "123".toFixed() 报错）。