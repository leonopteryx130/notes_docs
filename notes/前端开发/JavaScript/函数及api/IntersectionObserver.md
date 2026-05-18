# IntersectionObserver

在带滚动条的页面中，常常需要了解某个元素是否进入了“窗口（viewport）”，传统的实现方法是，监听到scroll事件后，调用目标元素（绿色方块）的getBoundingClientRect()方法，得到它对应于视口左上角的坐标，再判断是否在视口之内。这种方法的缺点是，由于scroll事件密集发生，计算量很大，容易造成性能问题。

现在的开发中出现了一个新的IntersectionObserver API，可以自动"观察"元素是否可见。由于可见（visible）的本质是，目标元素与视口产生一个交叉区，所以这个 API 叫做"交叉观察器"。

**用法**：

```
var io = new IntersectionObserver(callback, option);
```
IntersectionObserver是浏览器原生提供的构造函数，接受两个参数：callback是可见性变化时的回调函数，option是配置对象（该参数可选）。

当目标元素的可见性变化时，就会调用观察器的回调函数callback。函数callback一般会触发两次。一次是目标元素刚刚进入视口（开始可见），另一次是完全离开视口（开始不可见）。callback函数的参数（entries）是一个数组，每个成员都是一个IntersectionObserverEntry对象。举例来说，如果同时有两个被观察的对象的可见性发生变化，entries数组就会有两个成员。