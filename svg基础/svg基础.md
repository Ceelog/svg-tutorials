# svg 基础

SVG(Scalable Vector Graphics，可伸缩矢量图形)是一种相对新的 W3C 标准。当前 SVG 版本为 1.1，版本 2.0 在开发中。

## SVG 与 传统的位图相比有如下优势

- 比位图更小，下载速度更快。
- 缩放无不失真，适应不同的设备。
- SVG 在浏览器中构建，减少了服务器和网络传输的压力。
- 客户端可以与 SVG 进行交互并做修改，无需网络通信。
- ~~SVG 提供 SMIL 动画（自从 Chrome 45，SMIL 动画被弃用了）~~
- 能使用 JavaScript/CSS 操作 SVG。
- SVG 是文本，能被高效压缩。
- SVG 提供了很图片编辑效果，如遮罩、剪切、filter 等。

## 为什么 SVG 是一种 XML 语言(基于 XML 定义 SVG))

- 遵守代码编写及客户端渲染的规范。
- 如 XML 一样，SVG 也用描述文本表示，可读性高。
- 也是最重要的一点，可以使用 JavaScript 操作 SVG。

## SVG 坐标系

SVG 的坐标系与 HTML 一样。原点 `(0,0)` 在左上角。向右移动，x 增加。向下移动，y 增加。

![](./imgs/coord.png)

## SVG 的使用

SVG 的使用方式很多，常用的有如下几种

### `<img>`

```html
<img src="./pine.svg" alt="pine" />
```

### `background-image`

```css
.pine {
  background-image: url(./pine.svg);
}
```

### `<iframe>`

```html
<iframe src="./pine.svg">Your browser does not support iframe</iframe>
```

### `<embed>`

```html
<!-- Img URL -->
<embed type="image/svg+xml" src="./pine.svg" id="svg-embed" />
```

如果想获取当前 `<embed>` 标签的 SVG 文档，可以通过如下方式：

```js
document.getElementById('svg-embed').getSVGDocument();
```

### `<object>`

```html
<object type="image/svg+xml" data="./pine.svg">Your browser does not support SVG</object>
```

### 内联

- svg 标签内联
  ```html
  <svg height="512" viewBox="0 0 64 64" width="512" xmlns="http://www.w3.org/2000/svg">
    ...
  </svg>
  ```
- Data URLs
  ```html
  <!-- img -->
  <img src="data:image/svg+xml;<DATA>" alt="pine" />
  <!-- iframe -->
  <iframe src="data:image/svg+xml;<DATA>">Your browser does not support iframe</iframe>
  <!-- object -->
  <object data="data:image/svg+xml;<DATA>" type="image/svg+xml">Your browser does not support SVG</object>
  <!-- embed -->
  <embed type="image/svg+xml" src="data:image/svg+xml;<DATA>" id="svg-embed" />
  ```
  ```css
  .pine {
    background-image: url('data:image/svg+xml;<DATA>');
  }
  ```
  **内联引用方式可以有效地减少 http 的请求数，但不能很好地利用缓存，同时会增加文档大小。**

## 交互

SVG 与传统的位图相比最大的亮点在于可以通过 JavaScript/CSS 对 SVG 进行操作。通常如下两种情况都可以使用 JavaScript/CSS 与 SVG 进行交互，不过需要遵守同源策略。

- SVG 直接内联于 HTML 中
- SVG 通过`<iframe>`、`<object>`、`<embed>`加载

>singsong: 主流浏览器都能进行交互，具体依赖于浏览器的实现

特征              | 内联 | `<iframe>`、`<object>`、`<embed>` | `<img>`、`background-image`
---------------- | ---- | -------------------------------- | --------------------------:
交互              | ✅  | ✅                                | ❌
动画              | ✅  | ✅                                | ✅
交互动画          | ✅  | ✅                                | ❌
执行内联脚本       | ✅  | ✅                                | ❌
被外部脚本操作     | ✅  | ❌                                | ❌

## 定制样式
- attributes：`fill`, `fill-opacity`, `stroke`, `stroke-width` 等
  ```html
  <svg>
    <text x="5" y="30" fill="red">A nice text</text>
  </svg>
  ```
- style
  ```html
  <svg>
    <text x="5" y="30" style="fill: red">A nice text</text>
  </svg>
  ```
- css
  ```html
    <style>
        svg text{
          fill: red;
        }
    </style>
    <svg>
      <text x="5" y="30">A nice text</text>
    </svg>
  ```
**优先级：style > css > attributes**

## CSS 和 JavaScript 的引入方式

### [`CDATA`](http://www.w3.org/TR/REC-xml/#sec-cdata-sect) ([Character Data](http://www.w3.org/TR/REC-xml/#dt-chardata))

`CDATA` 表示那些不应该被解析为 XML 标签的字符数据。

`CDATA` 与注释的区别

- `CDATA` 是文档一部分，而注释不是
-  [在 `CDATA` 中不能包括`"]]>"` 特殊字符](http://www.w3.org/TR/REC-xml/#NT-CDEnd)，[而`"--"`在注释中是无效的](https://www.w3.org/TR/REC-xml/#sec-comments)。
- [字符实体](http://www.w3.org/TR/REC-xml/#dt-PERef)在注释中不能被解析

[更多详解--->](https://stackoverflow.com/questions/2784183/what-does-cdata-in-xml-mean)

### SVG 内部样式

为了避免 CSS 样式被解析为XML标签，需要将样式写在 `CDATA` 中
```html
<svg>
  <style>
    <![CDATA[
      #rect { fill: blue; }
    ]]>
  </style>
  <rect id="rect" x="0" y="0" width="10" height="10" />
</svg>
```
也可以在 SVG 内部引用 CSS 文件
```html
<?xml version="1.0" standalone="no"?>
<?xml-stylesheet type="text/css" href="style.css"?>
<svg xmlns="http://www.w3.org/2000/svg" version="1.1"
     width=".." height=".." viewBox="..">
  <rect id="my-rect" x="0" y="0" width="10" height="10" />
</svg>
```
### SVG 外部样式
```html
<style>
  #rect {
    fill: red
  }
</style>
<svg>
  <rect x="0" y="0" width="10" height="10" fill="blue" id="rect" />
</svg>
```
**优先级： SVG 内部样式 > SVG 外部样式**

### SVG 内部脚本

可以将 JavaScript 写 SVG 内部。同样，为了避免 JavaScript 被解析为 XML 标签，需要将 JavaScript 写在 `CDATA` 中
```html
<!-- 确保节点已准备好 -->
<svg>
  <script>
    <![CDATA[
      window.addEventListener('DOMContentLoaded', () => {
        //...
      }, false)
    ]]>
  </script>
  <rect x="0" y="0" width="10" height="10" fill="blue" />
</svg>
<!-- 或放置底部 -->
<svg>
  <rect x="0" y="0" width="10" height="10" fill="blue" />
  <script>
    <![CDATA[
      //...
    ]]>
  </script>
</svg>
```
### SVG 外部脚本
```html
<!-- 确保节点已准备好 -->
<script>
  window.addEventListener('DOMContentLoaded', () => {
    document.getElementById('rect').setAttribute('fill', 'red')
      }, false);
</script>
<svg>
  <rect id="rect" x="0" y="0" width="100" height="100" fill="blue" />
</svg>

<!-- 或放置底部 -->
<svg>
  <rect id="rect" x="0" y="0" width="100" height="100" fill="blue" />
</svg>
<script>
  document.getElementById('rect').setAttribute('fill', 'red')
</script>
```
### SVG vs Canvas

Canvas API 是 Web 平台的一个很好的补充，它具有与 SVG 类似的浏览器支持。与 SVG 的主要差异在于 Canvas 是基于像素的绘图。

|  | Canvas | SVG |
| -- | -- | --- |
| 历史 | 新兴，由 Apple 私有的技术发展而来  | 历史悠久，2003 年成为 W3C 标准  |
| 操作对象 | 基于像素  | 基于图形元素  |
| 驱动 | 脚本   | 脚本和 CSS  |
| 事件对象 | 像素点  | 图形元素  |


## 参考文章

- [SVG](https://www.w3.org/Graphics/SVG/IG/resources/svgprimer.html)
- [An in-depth SVG tutorial](https://flaviocopes.com/svg/)
- [svg应用指南](https://svgontheweb.com/zh/)
