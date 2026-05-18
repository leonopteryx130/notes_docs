# slot

**新版本API**：在2.6.0中，具名插槽和作用域插槽引入了统一的语法，v-slot指令，同时原有的slot和slot-scope指令已经被取代，不建议使用。

理解概念：Slot 通俗的理解就是“占坑”，在组件模板中占好了位置，当使用该组件标签时候，组件标签里面的内容就会自动填坑（替换组件模板中slot位置），通过配置也可以设置默认模板（即如果父组件调用时候，传入了相关的内容，就使用父组件内容，如果父组件没有传入就使用子组件的默认内容）

一个最简单的demo：

```
//父组件home.vue
<test>
     Hello Word
</test>

//子组件test.vue
<a href="#">
    <slot></slot>
</a>
```
子组件的slot会被替换为Hello Word字符串，结果如下：

```
//渲染结果
<a href="#">
    Hello Word
</a>
```

插槽也可以被替换为HTML元素，vue组件等。父组件的替换内容也支持使用变量。比如：

```
<test>
    //插槽可以获取到home组件里的内容
    Hello {{enhavo}}
</test>
data(){
    return{
        enhavo:'word'
    }
}
```

**关于插槽默认值**：

插槽也支持提供一个默认值：

```
//test.vue，子组件插槽里提供默认值
<button>
    <slot>Submit</slot>
</button>

//home.vue 父组件调用时候如果不提供默认值，最终结果展示子组件的默认值
<test></test>

//home.vue父组件如果提供了默认值，最终展示父组件的值
<test>按钮</test>
```

**具名插槽**：

很多时候，我们可能期望组件里有多个插槽，并且可以控制插槽渲染不同的内容，这个时候就需要使用slot元素特有的name属性。slot标签的name属性可以让插槽变成一个具名插槽，其实，如果没有name属性，也会有一个name="default"的值

举个例子，在子组件中这样定义：

```
<template>
    <div>
        <header>
            <slot name="header">父组件没有传入header</slot>
        </header>
        <main>
            <slot>父组件没有传入默认的slot，或者值为default的slot</slot>
        </main>
        <footer>
            <slot name="footer">父组件没有传入footer</slot>
        </footer>
    </div>
</template>
```

父组件这样调用：

```
<template>
    <ChildVue>
        <template v-slot:header>
            <h1>父组件传入的header</h1>
        </template>
        <p>父组件传入的content</p>
        <template v-slot:footer>
            <p>父组件传入的footer</p>
        </template>
    </ChildVue>
</template>
```

输出结果：

<div align="left">
    <img src=./slot.png width=30% />
</div>