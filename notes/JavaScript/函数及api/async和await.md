# async和await

### **async**：

async和await其实是promise的语法糖，async能实现的效果都能用promise+then链来实现，async其实是为了优化then链而开发出来的。async语法糖会默认把后边的函数包装在promise对象里返回，函数体内的内容会包装在promise的定义里，return的内容会包装在resolve里边，举个例子：

```javascript
async function f() {
    return 1;
}
f().then((res) => {
    console.log(res)
})
```
等价于：

```javascript
function f() {
    return new Promise((resolve, reject) => {
        resolve(1)
    })
}
f().then((res) => {
    console.log(res)
})
```

### **await**：

await关键字必须出现在async函数里，否则会报错，但是await关键字后边可以是任意值，如果await后边的表达式不是promise对象，那么await的返回值就是表达式运算的结果。如果await后边是一个promise对象，那么promise对象里得到的值就会作为await表达式的运算结果。

无论await关键字后边是不是promise对象，其后边的代码（在async函数里）都会添加在微任务里边。

async和await的作用，就是简化then链，让代码看起来更简洁，举个例子，假设一个业务，分多个步骤完成，每个步骤都是异步的，而且依赖于上一个步骤的结果。我们仍然用 setTimeout 来模拟异步操作：
```javascript
function takeLongTime(n){
  return new Promise((resolve) => {
    setTimeout(() => resolve(n + 200),n);
  })
}
function step1(n){
  console.log(`step1 with ${n}`);
  return takeLongTime(n);
}
function step2(n){
  console.log(`step2 with ${n}`);
  return takeLongTime(n);
}
function step3(n){
  console.log(`step3 with ${n}`);
  return takeLongTime(n);
}
```
用Promise链来实现
```javascript
function doIt(){
  let time1 = 300;
  step1(time1)
    .then((time2) => step2(time2))
    .then((time3) => step3(time3))
    .then((result) => {
      console.log(`result is ${result}`);
    })
}
    
doIt();
//执行结果为:
//step1 with 300
//step2 with 500
//step3 with 700
//result is 900
```
使用async和await实现：

```javascript
async function doIt() {
  let time1 = 300;
  let time2 = await step1(time1);//将Promise对象resolve(n+200)的值赋给time2
  let time3 = await step1(time2);
  let result = await step1(time3);
  console.log(`result is ${result}`);
}
doIt();
//执行结果为:
//step1 with 300
//step2 with 500
//step3 with 700
//result is 900
```
async和await成功解决了promise…then链传递参数麻烦的情况。但是还有一个重要的事情，就是promise可能有reject，但是await返回只会返回resolve的结果，因此，对于运行结果可能是reject的情况，最好把await放在try…catch代码块里执行