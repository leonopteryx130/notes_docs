# as关键字，类型断言

通常情况下，开发者会比TypeScript更清楚某个值的详细类型，TypeScript在编译的时候会先检查这个值的类型，这个行为需要一点时间，如果使用断言，就相当于直接告诉TypeScript这个值的类型，不用检查，TypeScript会假设开发者已经做了必要的检查。这样提高了编译速度，但是一定是开发者非常确定某个值的类型时候才可以使用。

类型断言由两种语法形式：
- 尖括号语法：
```
let someValue:any = 'this is a string'；
let strLength:number = (<string>someValue).length；
```

- as语法
```
let someValue:any = 'this is a string'；
let strLength:number = (someValue as string).length；
```

两种形式是等价的。至于使用哪个大多数情况是凭个人爱好；然而，当你在```TypeScript```里使用```JSX```时，只有 ```as``` 语法断言是被允许的。


**Tips**：当设置类型为any时，一般都可以用断言。