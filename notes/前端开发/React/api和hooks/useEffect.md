# useEffect

### **一、useEffect通常是render结束之后才执行的**
比如这段代码：
```
function A() {
  React.useEffect(() => {
    console.log("A");
  });
  return (
    <div className="A">
      <B />
    </div>
  );
}
function B() {
  React.useEffect(() => {
    console.log("B");
  });
  return (
    <div>
      B组件
    </div>
  );
}
```

会先打印出B再打印出A，因为执行A组件的时候会先执行render，render中调用了B组件，因此会先执行B组件中的代码，B组件render中没有调用其他组件，因此执行完B组件的render后，会执行B组件的useEffect，此时打印出B，然后继续执行A组件的render，直到A组件的render执行完后，再执行A组件的useEffect，此时打印出A。

### **二、 useEffect的return**
useEffect里边是可以执行return语句的，当组件销毁前会默认执行，或者当下一次useEffect调用的时候（比如监听的值改变导致再次调用时候），也会执行return语句，return语句一般来说是个函数，用来清除一些useEffect产生的副作用，比如清除定时器，不用的dom，被修改的状态等，合理运用return语句可以有效避免重复渲染等问题