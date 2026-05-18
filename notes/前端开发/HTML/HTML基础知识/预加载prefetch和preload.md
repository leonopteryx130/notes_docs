# 预加载prefetch和preload

## **预加载属性 preload 与 prefetch 区别：**

 - preload 告诉浏览器立即加载资源;
 - prefetch 告诉浏览器在空闲时才开始加载资源；
 - preload、prefetch 仅仅是加载资源，并不会“执行”;
 - preload、prefetch 均能设置、命中缓存；

通过这两种方式资源的加载都不会阻塞关键渲染路径；

使用 preload 会将资源优先级设置为 Highest，而使用 prefetch 会将资源优先级设置为 Lowest，Lowest 资源将会在网络空闲时才开始加载。
preload 优先级高于 prefetch；
一些帮助理解的实验：

实验一：
```
<!DOCTYPE html>
<head>
  <link rel="preload" href="/style.css?t=2000" as="style">
  <link rel="preload" href="/main.js?t=2000" as="script">
  <link rel="prefetch" href="/prefetch.css?t=1000" as="style">
  <link rel="prefetch" href="/prefetch.js?t=1000" as="script">
</head>
<body>
  <p>hello world</p>
  <p class="preload">preload</p>
  <p class="prefetch">prefetch</p>
</body>
</html>
```
 <div align="center">
    <img src=./预加载prefetch.png width=100% />
</div>

发现 JS 没有输出，CSS 也没有生效。也就是说通过 preload，prefetch 加载的资源并不会立即“执行”；

实验二：
```
<!DOCTYPE html>
<head>
  <link rel="preload" href="/preload.js?t=3000&ma=3600" as="script">
  <link rel="prefetch" href="/prefetch.js?t=3000&ma=3600" as="script">
</head>
<body>
  <p>hello world</p>
  <script>
    setTimeout(() => {
      if (location.href.indexOf('r') === -1) {
        location.href = `http://localhost:3000?r=${Date.now()}`
      }
    }, 1000)
  </script>
</body>
</html>
```
这段代码不会加载preload和prefetch资源，在使用 preload、prefetch 加载资源时，如果发生了页面跳转，此时没有完成的请求将会被取消掉；

实验三：
```
<!DOCTYPE html>
<head>
  <link rel="preload" href="/main.js?t=3000" as="script">
  <link rel="prefetch" href="/main.js?t=3000" as="script">
</head>
<body>
  <p>hello world</p>
  <script src="/main.js?t=3000"></script>
</body>
</html>

```

当页面中实际用到的资源与 preload、prefetch 加载的资源重复时，浏览器不会进行重复请求（这个例子中不会出现第三个 main.js 的请求）；
但是相同的资源重复使用 preload、prefetch 属性时将会导致重复下载（main.js 请求了两次，第二次耗时 6s 的请求为 prefetch 发起的）；