# margin-inline和padding-inline

## 背景

CSS 里常见的 `margin-left`、`padding-right` 等写法是**物理属性**：它们始终对应屏幕上的左、右、上、下，与文字阅读方向无关。

`margin-inline`、`padding-inline` 等属于**逻辑属性**：它们描述的是元素在**行内轴（inline axis）**和**块级轴（block axis）**上的间距，会随 `direction`、`writing-mode` 等书写模式自动映射到具体的物理方向。

这样在 LTR（从左到右）和 RTL（从右到左）布局下，同一份样式无需分别写 `left` / `right`，也能保持语义一致。

## 行内轴与块级轴

在默认的水平书写模式下：

- **行内轴（inline）**：文字排列的方向，通常是水平方向
- **块级轴（block）**：块级元素堆叠的方向，通常是垂直方向

| 逻辑方向 | 默认水平书写下对应 | 含义 |
| --- | --- | --- |
| inline-start | left | 行内起始侧 |
| inline-end | right | 行内结束侧 |
| block-start | top | 块级起始侧 |
| block-end | bottom | 块级结束侧 |

当 `direction: rtl` 时，`inline-start` 会映射到右侧，`inline-end` 映射到左侧，物理属性不会自动翻转，逻辑属性会。

## margin 对应关系

| 物理写法（固定方向） | 逻辑写法（跟随文字方向） | 说明 |
| --- | --- | --- |
| margin-left | margin-inline-start | 起始侧外边距 |
| margin-right | margin-inline-end | 结束侧外边距 |
| margin-left + margin-right | margin-inline | 同时设置行内轴两侧外边距 |
| margin-top | margin-block-start | 块级起始侧外边距 |
| margin-bottom | margin-block-end | 块级结束侧外边距 |
| margin-top + margin-bottom | margin-block | 同时设置块级轴两侧外边距 |

## padding 对应关系

padding 与 margin 使用同一套逻辑命名，只是把 `margin` 换成 `padding`：

| 物理写法（固定方向） | 逻辑写法（跟随文字方向） | 说明 |
| --- | --- | --- |
| padding-left | padding-inline-start | 起始侧内边距 |
| padding-right | padding-inline-end | 结束侧内边距 |
| padding-left + padding-right | padding-inline | 同时设置行内轴两侧内边距 |
| padding-top | padding-block-start | 块级起始侧内边距 |
| padding-bottom | padding-block-end | 块级结束侧内边距 |
| padding-top + padding-bottom | padding-block | 同时设置块级轴两侧内边距 |

## 简写语法

`margin-inline` 和 `padding-inline` 的简写规则与 `margin`、`padding` 类似：

```css
/* 两侧相同 */
margin-inline: 16px;

/* inline-start | inline-end */
margin-inline: 8px 24px;

/* 分别设置 */
margin-inline-start: 8px;
margin-inline-end: 24px;
```

`margin-block`、`padding-block` 同理，只是作用在块级轴上。

## 示例

### LTR 下等价关系

默认 `direction: ltr` 时：

```css
.box {
  margin-inline: 10px 20px;
}

/* 等价于 */
.box {
  margin-left: 10px;
  margin-right: 20px;
}
```

### RTL 下自动翻转

```css
.box {
  direction: rtl;
  margin-inline: 10px 20px;
}

/* 等价于 */
.box {
  direction: rtl;
  margin-right: 10px;  /* inline-start 在右侧 */
  margin-left: 20px;   /* inline-end 在左侧 */
}
```

若使用 `margin-left: 10px; margin-right: 20px`，在 RTL 下左右含义不会随文字方向变化；使用 `margin-inline` 则 start/end 会跟着阅读方向走。

## 使用建议

- 需要适配多语言、RTL 布局时，优先使用 `margin-inline`、`padding-inline` 等逻辑属性。
- 只面向固定 LTR、且不涉及国际化时，物理属性与逻辑属性效果相同，可按团队习惯选择。
- 设置单侧间距时，用 `-inline-start` / `-inline-end` 比 `-left` / `-right` 语义更清晰，也更利于维护。
