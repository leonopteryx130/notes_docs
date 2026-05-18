# Performance

Performance API提供了一系列接口用于检测和访问浏览器页面的性能数据，这些数据和指标大多数都是浏览器自动收集的，只需要调用api接口和方法检索和访问即可。

#### 一个最基本例子：

官方推荐的首选方法是使用PerformanceObserver接口，该方法接受一个回调函数作为参数，PerformanceObserver接口返回一个PerformanceObserver对象，对象有三个方法，其中observe方法最常用，用于指定检测是entryTypes，可以指定多个entryTypes。用法：

```
const myObserver = (entrylist: Array<any>) => {
  entrylist.forEach((entry) => {
      console.log(entry.name)
  })
}
const perfObserver = new PerformanceObserver((entryList) => {
    myObserver(entryList.getEntries())
})
perfObserver.observe({ entryTypes: ['paint', 'first-input'], buffered: true })
```

通常情况下，直接这样调用会显得代码比较乱，需要进行一个封装：

```
export type IPerCallback = (entries: PerformanceEntry[]) => void
const getObserver = (type: Array<string>, callback: IPerCallback) => {
    /*
    封装PerformanceObserver和observe
    */
    const perfObserver = new PerformanceObserver((entryList) => {
        callback(entryList.getEntries())
    })
    perfObserver.observe({ entryTypes: [...type], buffered: true })
}
export default getObserver
```

#### 一些基本概念：
**enterType**：一个只读的字符串，用于表示性能指标的一个条目，每个条目对应一个性能指标，每个性能指标有自己的一系列参数。这个字符串在代码中通常是observe方法里的entryTypes参数所对应的字符串，举几个例子：
- paint：关于前端document渲染的性能指标（包括FP，FCP等）

**PerformanceObserverEntryList**：这个类型通常出现在PerformanceObserver方法的回调函数的参数中，也就是上边例子中的entryList，这个类型通过getEntries方法即可得到一个数组，数组中的数据类型是根据entryType来决定的（比如PerformanceEventTiming，LargestContentfulPaint类型），在MDN上，可以先查找entryType，得到如下结果：

<div align="center">
    <img src=./Performance1.png width=80% />
</div>

结果页中有很多entryType可以取到的值，每个值都有自己接口对应的对象，这个对象就是getEntries方法得到的数组的类型，对边打开一个类型的连接，可以看到有很多的只读属性，如下图：

<div align="center">
    <img src=./Performance2.png width=80% />
</div>

上边例子代码中，entry.name就是这些只读属性的其中之一，其实就是上边图片中的PerformanceEntry.name。

注：具体Performance的封装和使用案例见[代码仓库](https://github.com/leonopteryx130/code_demo/tree/main/solution/performancetest)