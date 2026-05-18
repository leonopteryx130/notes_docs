# canvas获取渲染上下文

在canvas元素对象上调用```getContext```方法即可获取canvas的渲染上下文，```getContext```方法支持输入一个字符串参数，这个字符串参数表示上下文类型，目前只有 2d 这个参数是全部浏览器所支持的。

***

### getContext方法签名：

canvas.getContext(contextType, contextAttributes);

- contextType：上下文类型

- contextAttributes：上下文参数

***

假设有一个定义好的 canvas:

```
<canvas id="my-house" width="300" height="300"></canvas>
```

### canvas获取2d渲染上下文的做法：

```contextType```的值为 2d

```
var ctx =canvas.getContext("2d");
```

2d上下文可以配置一个参数：

- **alpha**: 
  
  boolean值，表明canvas包含一个alpha通道. 如果设置为false, 浏览器将认为canvas背景总是不透明的, 这样可以加速绘制透明的内容和图片.

写法：

```
canvas.getContext("2d", { alpha: true});
```

### 在目前一些主流浏览器里也支持创建3d上下文:

contextType取值:

- "webgl" (或"experimental-webgl") 这将创建一个 WebGLRenderingContext 三维渲染上下文对象。只在实现WebGL 版本 1(OpenGL ES 2.0) 的浏览器上可用。

- "webgl2" (或 "experimental-webgl2") 这将创建一个 WebGL2RenderingContext 三维渲染上下文对象。只在实现 WebGL 版本 2 (OpenGL ES 3.0) 的浏览器上可用。

### getContext还支持单纯处理图片功能

设置```contextType```的值为```bitmaprenderer```
它创建一个```ImageBitmapRenderingContext```渲染对象，可以将```ImageBitmap```对象的内容传入canvas中。

用法：

```
function loadImage(src) {
  const image = new Image();
  return new Promise((resolve) => {
    image.onload = function() {
      resolve(image);
    }
    image.src = src;
  });
}
(async function() {
  const image = await loadImage(url);
  const bitmap = createImageBitmap(image, 0, 0, 32, 32);
  const canvas = document.querySelector('canvas');
  const context = canvas.getContext('bitmaprenderer');
  context.transferFromImageBitmap(bitmap);
  ...
}())
```

因为```bitmaprenderer```是可以用于worker上下文中的，所以这是一种快速让位图数据在worker和主线程中通讯的方式。