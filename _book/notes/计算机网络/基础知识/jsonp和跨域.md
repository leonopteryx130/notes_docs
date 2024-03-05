# jsonp和跨域

**背景知识**：跨域问题要保护的对象是客户端，客户端浏览器收到的通常是一个html并且包含一些脚本文件（就是各种js文件），这个html需要加载各种资源文件，如果资源或者脚本与html不是同一个源（比如图片和数据通常来源于后端代码和数据库），那么就不确定这个资源有没有危险，会不会对客户端有害，因此要加同源策略限制。

但是html中的script标签比较特殊，它请求的是一个javascript脚本并嵌入了html中，所以这段脚本中带回来的数据和资源与当前html相当于是同一个域，因此不受同源策略影响。因此这种方式也可以用来解决跨域问题，需要前端写一个放入数据的回调函数让服务端来配合执行，这种方式就是jsonp。

## **demo coding**:
假设在远程服务器```http://remoteserver.com```的根路径下有如下js代码：
```
localHandler({
    "result":"我是远程js带来的数据"
});
```
在本地html中：
```
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <script type="text/javascript">
        var localHandler = function(data){
            alert('我是本地函数,可以被跨域的remote.js文件调用,远程js带来的数据是:' + data.result);
        };
        </script>
        <script type="text/javascript" src="http://remoteserver.com/remote.js"></script>
    </head>
    <body></body>
</html>
```

在这个html上可以成功展示服务端的数据。