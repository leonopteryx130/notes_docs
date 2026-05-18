# setTimeout的知识

setTimeout函数是异步代码，但其实setTimeout并不是真正的异步操作，setTimeout的本质是属于宏任务。在执行的时候（也就是说setTimeout的时间已经过了），setTimeout会将回调函数里边的代码放到整个任务队列的末尾去执行，如果setTimeout设置的时间为0，就会立刻加入任务队列末尾，而不是立即执行，举个例子：

```
var t = true;
    
window.setTimeout(function (){
    t = false;
},1000);
    
while (t){}
    
alert('end');
```
这段代码中，alert是永远不会执行的，t也不会变为false，因为js的工作机制是当任务队列中没有任何同步代码的时候才会执行异步代码，setTimeout是异步代码，所以 setTimeout 只能等JS空闲才会执行，但死循环是永远不会空闲的，所以 setTimeout 也永远得不到执行。再举个类似的例子：

```
var start = new Date();
    
setTimeout(function(){  
    var end = new Date();  
    console.log("Time elapsed: ", end - start, "ms");  
}, 500);  
      
while (new Date - start <= 1000){}
```

输出结果一定是大于1000的，因为setTimeout 异步代码 其回调函数执行必须需等待主线程运行完毕，当while循环因为时间差超过 1000ms 跳出循环后，setTimeout 函数中的回调才得以执行