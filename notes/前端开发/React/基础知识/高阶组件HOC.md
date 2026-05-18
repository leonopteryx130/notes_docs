# 高阶组件HOC

在 React 中，高阶组件（HOC）是一个接收组件作为参数并返回一个新组件的函数。换句话说，它是一种组件的转换器。高阶组件通常用于在组件之间复用逻辑，例如状态管理、数据获取、访问控制等。

### 先实现一个高阶组件：

```
import React from "react";

function withLoading(WrappedComponent) {
  return function WithLoadingComponent({ isLoading, ...props }) {
    if (isLoading) {
      return <div>Loading...</div>;
    } else {
      return <WrappedComponent {...props} />;
    }
  };
}

export default withLoading;
```
在上述代码中，我们定义了一个 ```withLoading``` 函数，它接受一个组件作为参数```WrappedComponent```，并返回一个新的组件 ```WithLoadingComponent```。新组件接收一个名为 ```isLoading``` 的属性，如果 ```isLoading``` 为 ```true```，则显示一个加载指示器；否则，渲染 ```WrappedComponent```。

### 用法：

```
import React from "react";
import withLoading from "./withLoading";

function DataList({ data }) {
  return (
    <ul>
      {data.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
}
const DataListWithLoading = withLoading(DataList);

export default DataListWithLoading;
```
在这个示例中，我们首先导入了 ```withLoading``` 高阶组件，然后使用它来包装我们的 ```DataList``` 组件。这将创建一个名为 ```DataListWithLoading``` 的新组件，它在加载状态下显示一个加载指示器，否则显示数据列表。
```
import DataListWithLoading from "./DataListWithLoading";

function App() {
  return (
    <div>
      <DataListWithLoading data={['data1', 'data2']} isLoading={true} />
    </div>
  );
}

export default App;
```
我们将 ```data``` 和 ```isLoading``` 作为属性传递给 DataListWithLoading 组件。当数据正在加载时，组件将显示加载指示器

### 什么时候该用高阶组件呢？

就是希望给某一部分不同的组件加一些通用功能的时候，这个时候就可以考虑用高阶组件了，可以参考mixin设计模式的用途。

比如，需要根据用户权限决定组件是否需要隐藏，并且这个隐藏的需求并不固定，经常有组件需要添加或者取消这个功能，就可以写一个高阶组件来包装不同的组件，把是否隐藏的逻辑写在高阶组件里，这样可以少写很多代码（减小项目体积，试想，把是否隐藏的逻辑换成一个很复杂的逻辑），并且可以给用该高阶组件封装过的组件起一个名字，比如```withHiddenXXX```，做好组件命名规范的文档，之后只要遇到withHidden前缀的组件，就明白了是该组件包含了这个高阶组件的逻辑，对于代码可读性有很大帮助（前提是文档需要写好，对于开发者需要有一定经验）

