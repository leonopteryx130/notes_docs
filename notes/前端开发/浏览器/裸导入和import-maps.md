# 裸导入和 Import Maps

### 什么是裸导入？

裸导入（bare specifier）指 `import` 时的模块标识符**不以** `/`、`./`、`../` 开头，也不是完整 URL，例如 `import { createApp } from "vue"`。浏览器原生无法解析这种写法，只会报错。

前端项目里通常借助 Webpack、Vite 等构建工具：打包时从 `node_modules` 解析模块名，把裸名替换成真实路径，或直接打进 bundle，从而让浏览器能正常加载。

```js
// 我们编写的代码
import { createApp } from "vue";

// 构建后（示意）：裸名被解析为可访问的路径，或被打进 bundle
import { createApp } from "/assets/vue-xxx.js";
```

### Import Maps

Chrome 89+ 起支持 Import Maps（Firefox 108+、Safari 16.4+ 等现代浏览器也已支持）。通过 `<script type="importmap">` 声明模块标识符到 URL 的映射，即可在浏览器中直接使用裸导入，无需构建工具解析。

Import Maps 使用 JSON 定义映射关系，且必须写在依赖它的 module 脚本之前：

```html
<script type="importmap">
{
  "imports": {
    "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js",
    "mylib": "/lib/mylib.js"
  }
}
</script>

<script type="module">
  // 解析为 https://unpkg.com/vue@3/dist/vue.esm-browser.js
  import { createApp, h } from "vue";

  // 解析为 /lib/mylib.js
  import mylib from "mylib";

  createApp({ render: () => h("h1", "Hello Vue3!") }).mount("#app");
</script>
```

注意：
- `type="importmap"` 的脚本内容必须是合法 JSON，写非 JSON 代码会报错
- 使用 `import` 的脚本必须带 `type="module"`
