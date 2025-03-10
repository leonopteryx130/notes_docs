# hash路由和history路由

### 路由需要有的作用：

1. 当浏览器地址变化时，切换页面；
2. 点击浏览器后退，前进按钮，网页内容发生变化；
3. 刷新浏览器，页面加载内容对应当前路由对应的地址；

在单页面web网页中，单纯的浏览器地址改变，网页不会重载，如单纯的hash值改变，网页是不会变化的，因此我们的路由需要监听事件，并利用js实现动态改变网页。hash路由和history路由就是实现这一功能的不同方式。

- hash 模式：监听浏览器地址hash值变化，并执行相应的js切换。
- history 模式：利用H5 history API实现url地址改变，网页内容改变。

### hash路由

使用window.location.hash 属性和window.onhashchange事件。可以监听浏览器hash值得变化，去执行相应的js切换网页。

hash路由实现原理：

1. hash 指的是地址中 # 号以及后面的字符。称为散列值。
2. 散列值不会随请求发送到服务器端的，所以改变hash,不会重新加载界面。
3. 监听onhashchange事件，hash改变时，可以通过window.location.hash来获取和设置hash值。
4. location.hash值的变化直接反应在浏览器的地址栏。

### history路由

history api的本质是操作window.history对象，这个对象实际上是一个栈结构，记录了浏览器的历史记录。

history路由实现原理：

1. history.back: 返回上一页，等价于history.go(-1)
2. history.forward: 进入下一页，等价于history.go(1)
3. history.go: 进入指定的页面，接受一个整数作为参数，正数表示进入下一页，负数表示进入上一页。如果是0，则刷新当前页面。
4. history.pushState: 添加一个新的历史记录。接受三个参数：state对象、页面标题（目前被忽略）、URL。相当于往浏览器的历史记录栈中添加一个新纪录，不会刷新页面。
5. history.replaceState: 替换当前的历史记录。接受三个参数：state对象、页面标题（目前被忽略）、URL。相当于修改浏览器历史记录栈中最上层的记录，也就是当前纪录，同样不会刷新页面。
6. 每当history对象发生变化，就会触发popstate事件。可以通过window.onpopstate事件监听history对象的变化。

### hash路由和history路由的一些区别

1. 路由地址表现形式不同：hash路由地址有#，history路由地址没有#。举个例子：hishtory路由地址为：```http://localhost:8080/home```。hash路由地址为：```http://localhost:8080/#/home```。
2. 刷新页面时，hash路由地址不会发送请求，history路由地址会发送请求。因此，history路由地址需要服务器配合（比如nginx），否则刷新页面会404。