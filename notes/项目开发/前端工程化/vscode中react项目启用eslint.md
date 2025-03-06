# vscode中react项目启用eslint

### 官方文档

https://eslint.nodejs.cn/docs/latest/use/getting-started

### 准备插件和安装依赖

- 安装vscode插件：ESLint
  <div align="left">
      <img src=./vscode中react项目启用eslint.jpg width=40% />
  </div>
- 安装依赖：
  并不需要手动一个一个安装，只需要执行命令：
  ```
  npm init @eslint/config@latest
  ```
  然后按照提示进行选择即可，最终会生成一个```eslint.config.mjs```文件，里面包含了所有配置项，可以根据需要进行修改。

### 初始化的 eslint.config.mjs 文件

```javascript
import globals from "globals";
import pluginJs from "@eslint/js";
import pluginReact from "eslint-plugin-react";


/** @type {import('eslint').Linter.Config[]} */
export default [
  {files: ["**/*.{js,mjs,cjs,jsx}"]},
  {languageOptions: { globals: globals.browser }},
  pluginJs.configs.recommended,
  pluginReact.configs.flat.recommended,
];
```

### 如何自定义配置

```javascript
import globals from "globals";
import pluginJs from "@eslint/js";
import pluginReact from "eslint-plugin-react";


/** @type {import('eslint').Linter.Config[]} */
export default [
  {files: ["**/*.{js,mjs,cjs,jsx}"]},
  {languageOptions: { globals: globals.browser }},
  {
    ignores: [
      ".webpack/",
      "webpack/",
      "node_modules/"
    ],
    rules: {
      // 在这里添加您自定义的规则
      "no-unused-vars": "error",
    },
  },
  pluginJs.configs.recommended, // 这行建议删去
  pluginReact.configs.flat.recommended,
];
```

> Tips: ```pluginJs.configs.recommended``` 这一行会导致一些自定义配置不生效，所以这一行通常可以删去