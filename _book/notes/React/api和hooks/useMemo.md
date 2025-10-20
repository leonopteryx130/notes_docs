# useMemo

**基本用法**：

useMemo(create,deps)函数通常传入2个参数，第1个参数为我们定义的一个“包含复杂计算且有返回值的函数”（注意，useMemo有返回值，这一点和useEffect稍有区别），第2个参数为该处理函数中存在的依赖变量，请注意凡是处理函数中有的数据变量都需要放入deps中。如果处理函数没有任何依赖变量，可以传入一个空数组[]。

**代码形式**：

```
const xxxValue = useMemo(() => {
    let result = xxxxx;
    //经过复杂的计算后
    return result;
}, [xx]);
```

**说明**：

1. 使用useMemo()将计算函数包裹住，将计算函数中使用到的数据变量作为作为第2个参数。
2. 计算函数体内，把计算结果以 return 形式返回出去。
3. xxxValue 为该函数返回值。

**使用示例**：若某React组件内部有2个number类型的变量num，random，有2个button，点击之后分别可以修改num，random的值。 与此同时，该组件中还要求显示出num范围内的所有质数个数总和。

**补充说明**：加入random纯粹是为了引发组件重新渲染，方便我们查看到useMemo是否启了作用。

**使用场景**：

useMemo和useCallback很相似，因为js的对象和函数（函数本质也是对象）都是引用类型，所以即使值（或者函数代码）一模一样，再重新定义的时候，也会被认为是一个完全不同的对象或者函数。因此需要useMemo来实现对象的“记忆化”，只要依赖项没有变化，就不会返回一个新的对象。搭配React.memo使用可以防止子组件进行不必要的渲染

```
import { useMemo, useState } from "react";
import { Child } from "./child";
export const Parent = () => {
  const [count, setCount] = useState(0);
  const increment = () => setCount(count + 1);
  const userInfo = useMemo(() => ({ name: "小明", age: 18 }), []);
  return (
    <div>
      <button onClick={increment}>点击次数：{count}</button>
      <Child userInfo={userInfo} />
    </div>
  );
};
```
说明，Child可以认为是一个子组件，有一个输入参数userInfo
