# 基于 symbol,use 元素的 svg sprite方案

## icon 发展历程
### 1. 为了减少 http 请求数，有了 css sprite。它是基于 CSS 的 `background-img`、 `background-position`、 `background-size`来控制特定 icon 的呈现。这种方案增减 icon 不是很方便(可以使用一些辅助工具来完成，如[spritesmith](https://github.com/Ensighten/spritesmith))，而且 icon 会存在模糊虚化等品质问题。

### 2. Data URIs，一种为了优化性能的技术方案。它可以将 icon 以 Data URIs 形式直接内联于 css 或 html 中，能避免额外的 http 请求。但是会增加文件的大小，复用性差。

```js
.icon{
background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mP8+h8AAu8B9totwrcAAAAASUVORK5CYII=);
}
```

### 3. Iconfont，将 Icon 定义为矢量字体, 在 CSS 中用@font-face 引入 Icon Font 自定义字体, 再利用 font-family 和 Unicode 字符码显示出指定的图标。更多可以参考阿里的[iconfont](http://www.iconfont.cn/)

```css
// 定义font-face
@font-face {
  font-family: 'icons';
  src: url('icons.woff?') format('woff'), /* chrome, firefox */
       url('icons.ttf?') format('truetype'); /* chrome, firefox, opera, Safari, Android, iOS 4.2+*/
  font-weight: normal;
  font-style: normal
}
// 定义样式
.iconfont{
  font-family:"iconfont" !important;
  font-size:16px;font-style:normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
```
```html
// Unicode引用
<i class="iconfont">&#x33;</i>
// 兼容性最好，支持ie6+，及所有现代浏览器。
// 支持按字体的方式去动态调整图标大小，颜色等等。
// 但是因为是字体，所以不支持多色。只能使用平台里单色的图标，就算项目里有多色图标也会自动去色。
```

上述 Unicode 引用存在 Unicode 书写不直观，语意不明确的问题。

```css
// class-font定义
.icon-xxx:before{
  content: '&#x33;';
  color: #222;
}
```
```html
// class-font引用
<i class="iconfont icon-xxx"></i>
// 兼容性良好，支持ie8+，及所有现代浏览器。
// 相比于unicode语意明确，书写更直观。可以很容易分辨这个icon是什么。
// 因为使用class来定义图标，所以当要替换图标时，只需要修改class里面的unicode引用。
// 不过因为本质上还是使用的字体，所以多色图标还是不支持的。
```

但是 iconfont 仅仅支持单色，且高分辨率下的显示效果不佳；iconfont 作为字体进行显示时，其显示的大小、位置都可能会受到 `font-size`,`line-height`, `word-spacing` 等 css 属性影响，其容器的 css 样式也会可能影响到该字体 icon 的位置等。另外，浏览器将 iconfont 视为文字进行抗锯齿优化，不同系统下对文字的渲染显示效果可能不同。

### 4. svg icon

#### 4.1. 使用 svg 方式

* img 标签

```js
<img src="./demo.svg" />
// 注意：无法显示SVG内嵌的<image>.
```

* embed 标签

```js
<embed src="./demo.svg" />
```

* object 标签

```js
<object type="image/svg+xml" data="./demo.svg">
  <param name="src" value="./demo.svg" >
</object>
```

* background-image

```js
<div style="background-image: url(./demo.svg);" />
// 注意：无法显示SVG内嵌的<image>.
```

* picture 标签

```js
<picture>
  <source srcset="./demo.svg"  type="image/svg+xml">
  <img src="./demo.png">
</picture>
// 注意：无法显示SVG内嵌的<image>.
```

#### 4.2. inline svg

将 svg 直接内联于 html 或 css 中，该方案可以有效地减少 http 的请求，但复用性差。

```js
<svg version="1.1"  xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="red" />
</svg>
// 或 data URIs
.icon{
  background: url(data:text/svg+xml;base64,<base64 encoded data>)
}
```

#### 4.3. 传统 svg sprite

该方案工作原理类似于 css sprite，区别在于合并的是 svg，和不存在模糊虚化等品质问题。不过该方案在定制样式上不是很不方便。Don't be worried! 接下来介绍的方案可以很好地解决该问题。

#### 4.4. 基于 symbol,use 元素的 svg sprite

该方案的工作原理是使用`symbol`元素封装 icon 元件，再通过`use`元素将其实例化引用。这种方案不仅灵活，方便，而且合成效率高，具备 svg 不失真特性，同时还可以定制 icon 样式。

**相关元素介绍：**

* `<g>`(group, 分组)元素：用来对相关图形元素进行分组，以便统一操作，比如旋转，缩放或者添加相关样式等。`<g>`定义的内容直接会呈现。
* `<defs>`元素：定义所有需要再次使用的引用元素，在 defs 元素中定义的图形元素不会直接呈现。
* `<symbol>`元素：定义一个图形模板对象。symbol 元素本身是不呈现的，只有 symbol 元素的实例（即一个引用了 symbol 的 `<use>`元素）才能呈现。`<symbol>`兼具`<g>`的分组功能和`<defs>`初始不可见的特性。`<symbol>`能够创建自己的视窗，所以能够应用 viewBox 和 preserveAspectRatio 属性。
* `<use>`元素：可以在 SVG 图像中多次重用一个预定义的 SVG 图形，包括`<g>`元素和`<symbol>`元素。被重用的图形可以在定义`<defs>`的内部（图形将不可见直到使用 use 来引用它）或外部。

**实例：**

```html
<svg style="height:0;width:0;display:none;" version="1.1" xmlns="http://www.w3.org/2000/svg">
  <!-- 定义两个icon -->
  <symbol id="icon-rect" viewBox="0 0 2 2">
    <rect width="2" height="2" x="0" y="0" fill="blue" />
  </symbol>
  <symbol id="icon-circle" viewBox="0 0 2 2">
    <circle cx="1" cy="1" r="1" fill="red" />
  </symbol>
</svg>
<!-- 引用 -->
<svg><use xlink:href="#icon-rect"/></svg>
<svg><use xlink:href="#icon-circle"/></svg>
```
定制样式：
```css
.icon{
  width: 100px;
  height: 100px;
}
// 这里是基于`fill: inherit`可继承特性，对icon的颜色做定制
.icon.icon-yellow{
  fill: yellow; 
}
```
```html
<svg style="height:0;width:0;display:none;" version="1.1" xmlns="http://www.w3.org/2000/svg">
  <symbol id="icon-rect" viewBox="0 0 2 2">
    <rect width="2" height="2" x="0" y="0" />
  </symbol>
  <symbol id="icon-circle" viewBox="0 0 2 2">
    <circle cx="1" cy="1" r="1" />
  </symbol>
</svg>
<svg class="icon"><use xlink:href="#icon-rect"/></svg>
<svg class="icon icon-yellow"><use xlink:href="#icon-circle"/></svg>
```

**使用方式：**

a. 将合成好的 svg sprite 内联到 html 中，作为 body 元素第一个孩子节点。

```html
<body>
  <svg style="height:0;width:0;display:none;" version="1.1" xmlns="http://www.w3.org/2000/svg">
    <!-- 定义两个icon -->
    <symbol id="icon-rect" viewBox="0 0 2 2">
      <rect width="2" height="2" x="0" y="0" fill="blue" />
    </symbol>
    <symbol id="icon-circle" viewBox="0 0 2 2">
      <circle cx="1" cy="1" r="1" fill="red" />
    </symbol>
  </svg>
  <!-- 引用 -->
  <svg><use xlink:href="#icon-rect"/></svg>
  <svg><use xlink:href="#icon-circle"/></svg>
  <!-- 其他页面结构 -->
</body>
```

b. 将合成 svg sprite 保存为独立的 svg 文件，使用`svg文件路径#id`方式进行引用。

```html
<body>
  <svg><use xlink:href="/sprite.svg#icon-rect"/></svg>
  <svg><use xlink:href="/sprite.svg#icon-circle"/></svg>
</body>
```

或在 css 中引用

```css
.icon {
  background: url(/sprite.svg#icon-circle);
}
```
该引用方式比较优雅地将样式和结构有效地分离，便于做缓存处理。但是这种外链引用方式存在跨越问题，兼容性(IE9-10)等，不过可以将 svg 文件作为 js 文件载入来解决：

svg.js:

```js
const sprite = '
<svg style="height:0;width:0;display:none;" version="1.1" xmlns="http://www.w3.org/2000/svg">
  <!-- 定义两个icon -->
  <symbol id="icon-rect" viewBox="0 0 2 2">
    <rect width="2" height="2" x="0" y="0" fill="blue" />
  </symbol>
  <symbol id="icon-circle" viewBox="0 0 2 2">
    <circle cx="1" cy="1" r="1" fill="red" />
  </symbol>
</svg>';
document.body.insertAdjacentHTML("afterBegin", sprite);
```

index.html:

```html
<body>
  <!-- 引入svg.js，因为script标签不存在跨越限制，与jsonp的原理有点类似 -->
  <script src = "/svg.js"></script>
  <!-- 引用 -->
  <svg>
    <use xlink:href="#icon-circle"/>
  </svg>
</body>
```

如果觉得上述解决方法比较繁琐，可以使用 GitHub 上对应的处理库：[svg4everybody](https://github.com/jonathantneal/svg4everybody)，原理类似。它会在不支持外链svg的情况下，异步加载svg文件并注入到页面中，将引用方式转换为内联引用。

### 基于 webpack 处理 svg sprite
这里使用[svg-sprite-loader](https://github.com/kisenka/svg-sprite-loader)加载器来处理svg icon的合成。
```js
  {
    test: /\.svg$/,
    include: paths.svgs,
    loader: 'svg-sprite-loader',
    options: {
      runtimeCompat: true,
      esModule: false
    }
  }
```
注意：在webpack配置中，需要过滤掉`file-loader`或`url-loader`对需要合成的svg文件的处理。
```js
  {
    exclude: [/\.(js|jsx|mjs)$/, /\.html$/, /\.json$/, paths.svgs],
    loader: require.resolve('file-loader'),
    options: {
      name: 'static/media/[name].[hash:8].[ext]',
    },
  },
```
一般通过软件(如illustrator)制作的svg，都会含有大量的冗余无用信息，这里可以通过[svgo-loader](https://github.com/rpominov/svgo-loader)进行优化处理。

### 总结
基于 symbol,use 元素的 svg sprite方案，与文章提到其他解决方案有着明显的优势：
- 具备矢量图缩放不失真的特性
- 可灵活自定制样式
- 合成体积小，管理方便

虽然现在还存在一些兼容性问题，不过对于手机端完全可以忽视。另外，因为svg是通过公式计算来的，所以渲染性能上不及图片。不过这些都影响不大，基于 symbol,use 元素的 svg sprite方案必是将来最佳icon合成方案。