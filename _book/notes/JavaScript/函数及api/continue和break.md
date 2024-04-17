# continue和break

### **break**:

break语句可以使流程跳出switch语句体，也可以用break语句在循环结构终止本层循环体，从而提前结束本层循环。

使用说明：
1. 只能在循环体内和switch语句体内使用break；
2. 当break出现在循环体中的switch语句体内时，起作用只是跳出该switch语句体，并不能终止循环体的执行。若想强行终止循环体的执行，可以在循环体中，但并不在switch语句中设置break语句，满足某种条件则跳出本层循环体。

### **continue**：

continue语句的作用是跳过本次循环体中余下尚未执行的语句，立即进行下一次的循环条件判定，可以理解为仅结束本次循环。

注意：continue语句并没有使整个循环终止。

### **举个例子**：
```
let fruit=["apple", "banana", "orange", "pear"]
for (let i = 0; i < fruit.length; i++) {
    switch (fruit[i]) {
        case "orange":
            console.log("嘿嘿嘿");
            break;
            // continue;
        default:
            break;
    }
    console.log("we have " + fruit[i]);
}
```

由于break只是跳出switch结构体，因此break不影响最后的console的执行