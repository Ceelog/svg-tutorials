# svg 描边动画

在开始讲解之前，先来看看效果

![svg](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/svg.gif)

## svg 描边属性 stroke

在 svg 中对图形（line、polyline、rect、circle、ellipse、polygon）、路径（path）、文本(text、textPath、tspan)的描边都需要用到`stroke`属性。

### stroke 常用属性

- stroke：颜色。
  ```html
  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <g>
      <path stroke="red" d="M0 20 l200 0" />
      <path stroke="green" d="M0 40 l200 0" />
      <path stroke="blue" d="M0 60 l200 0" />
    </g>
  </svg>
  ```
  ![stroke](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/stroke.png)

- stroke-width：宽度。
  ```html
  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <g stroke="black">
      <path stroke-width="2" d="M0 20 l200 0" />
      <path stroke-width="6" d="M0 40 l200 0" />
      <path stroke-width="12" d="M0 60 l200 0" />
    </g>
  </svg>
  ```
  ![stroke-width](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/stroke-width.png)

- stroke-linecap：线端点样式。取值`butt`、`round`、`square`，默认`butt`。
  ```html
  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <g stroke="black" stroke-width="6">
      <path stroke-linecap="butt" d="M10 20 l210 0" />
      <path stroke-linecap="round" d="M10 40 l210 0" />
      <path stroke-linecap="square" d="M10 60 l210 0" />
    </g>
  </svg>
  ```
  ![stroke-linecap](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/stroke-linecap.png)
- stroke-opacity：透明度。
  ```html
  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <g stroke="black" stroke-width="6">
      <path stroke-opacity="0.2" d="M10 20 l210 0" />
      <path stroke-opacity="0.6" d="M10 40 l210 0" />
      <path stroke-opacity="1" d="M10 60 l210 0" />
    </g>
  </svg>
  ```
  ![stroke-opacity](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/stroke-opacity.png)
  
- stroke-linejoin：转角处样式。取值`arcs`、`bevel`、`miter`、`round`，默认`miter`。
  ```html
  <svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="400" height="300"
     viewBox="0 0 400 300">
    <g>
      <polyline stroke-linejoin="miter"
                points="20,100 60,30 100,100"
                stroke="black" stroke-width="30"
                fill="none" />

      <polyline stroke-linejoin="round"
                points="20,180 60,110 100,180"
                stroke="black" stroke-width="30"
                fill="none" />

      <polyline stroke-linejoin="bevel"
                points="20,260 60,190 100,260"
                stroke="black" stroke-width="30"
                fill="none" />

    </g>
  </svg>
  ```
  ![stroke-linejoin](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/stroke-linejoin.png)

- stroke-dasharray：定义虚线样式。取值是一个逗号或者空格分隔的数列。数列的第一个值表示dash大小、第二值表示两个dash之间空隙大小。如`stroke-dasharray:10, 2` 表示dash大小10、dash间隙2。如果提供数列是奇数个，则会重复一次形成偶数个。如`stroke-dasharray:10`等效于`stroke-dasharray:10, 10`或`stroke-dasharray:10, 2, 5`等效于`stroke-dasharray:10, 2, 5, 10, 2, 5`。

  ```html
  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <g stroke="black" stroke-width="6">
      <path stroke-dasharray="5,5" d="M10 20 l210 0" />
      <path stroke-dasharray="10,10" d="M10 40 l210 0" />
      <path stroke-dasharray="20,10,5,5,5,10" d="M10 60 l210 0" />
    </g>
  </svg>
  ```
  ![stroke-dasharray](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/stroke-dasharray.png)
  >singsong：也就是说 `stroke-dasharray` 取值数列以两个数值为一个单元划分，每个单元第一个值表示dash大小，第二值表示两个dash之间空隙大小。[演示实例](http://htmlpreview.github.io/?https://github.com/zhansingsong/fe-tutorials/blob/master/svg描边动画/demo/stroke-dasharray.html)

  ![dasharray](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/dasharray.gif)

- stroke-miterlimit：当`stroke-linejoin: miter`时，对 `miter` 进行限制。


> singsong: 样式属性优先级为 style > css > attribute。

## 如何让描边动起来

### stroke-dashoffset

这个属性用于指定 stroke-dasharray 开始的偏移量。也是本文重点介绍对象，理解该属性如何工作，就能很好地掌握 svg 描边动画。stroke-dashoffset 取值可以大于 0，也可以小于 0。[演示实例](http://htmlpreview.github.io/?https://github.com/zhansingsong/fe-tutorials/blob/master/svg描边动画/demo/stroke-dashoffset.html)。
- 取值大于 0

  ![dashoffset1](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/dashoffset1.gif)

- 取值小于 0。等效于`offset = s - |-offset| % s`。其中`offset`表示正取值，`s`表示`dasharray`的总和（如：dasharray: '100 50'，则s: 150）。

  ![dashoffset2](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/dashoffset2.gif)


### 动画原理

随着时间的变化，通过控制 `stroke-dashoffset` 来控制 `stroke-dasharray` 开始的偏移量的变化，以到达动画效果。[演示实例](http://htmlpreview.github.io/?https://github.com/zhansingsong/fe-tutorials/blob/master/svg描边动画/demo/animation_js.html)。
>singsong：通常会将 `stroke-dasharray` 设置为路径总长度。如总长度为`s`，`stroke-dasharray: s`或`stroke-dasharray: s s`。再将 `stroke-dashoffset` 取值从 `s` 变化到 `0`，就可实现从无到有的描边动画。

![animation](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/animation.gif)

## 动画的实现方式：[演示实例](http://htmlpreview.github.io/?https://github.com/zhansingsong/fe-tutorials/blob/master/svg描边动画/demo/animation_demo.html)

- css
  ```css
  .path {
    stroke-dasharray: 523.1047973632812;
    stroke-dashoffset: 0;
    animation: dash 2s linear 3s alternate infinite;
  }
  @keyframes dash {
      from {
          stroke-dashoffset: 523.1047973632812;
      }
      to {
          stroke-dashoffset: 0;
      }
  }
  ```
- js。需要使用 `SVGGeometryElement.getTotalLength()` 方法，获取路径的总长度。在 SVG 1.1 中，`getTotalLength()` 只存在 SVGPathElement 中，而基本图形是没有该方法的。不过在 SVG 2.0 中，基本图形也继承该方法。[如何检测SVG版本，可以参考这里。](https://stackoverflow.com/questions/26088839/how-do-i-know-if-my-browser-supports-svg-2-0)
> Moved pathLength attribute and `getTotalLength()` and `getPointAtLength()` methods from `SVGPathElement` to `SVGGeometryElement`。————[SVG 2 support in Mozilla
](https://developer.mozilla.org/en-US/docs/Web/SVG/SVG_2_support_in_Mozilla)

  ```js
  const path = document.querySelector('.path');
  // 获取总长度
  const totalLength = path.getTotalLength();
  // 设置 stroke-dasharray
  path.style.strokeDasharray = totalLength;
  // 设置 stroke-dashoffset
  path.style.strokeDashoffset = totalLength;

  let currentFrame = 0;
  let totalFrames = 160;
  const draw = function() {
      let progress = currentFrame / totalFrames;
      if (progress > 1) {
          window.cancelAnimationFrame(handle);
      } else {
          currentFrame++;
          // 更新 stroke-dashoffset
          path.style.strokeDashoffset = totalLength * (1 - progress);
          handle = window.requestAnimationFrame(draw);
      }
  };
  draw();
  ```

## 实战

将 stroke 动画用于预加载动画展示

- [Demo](http://htmlpreview.github.io/?https://github.com/zhansingsong/fe-tutorials/blob/master/svg描边动画/demo/exercise.html)

  ![exercise](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/exercise.gif)

- [实战实例](https://tympanus.net/Development/SVGDrawingAnimation/index.html)

  ![inaction](https://raw.githubusercontent.com/zhansingsong/fe-tutorials/master/svg%E6%8F%8F%E8%BE%B9%E5%8A%A8%E7%94%BB/imgs/inaction.gif)

## 工具
- [vectr](https://vectr.com/)：SVG 在线制作工具——[Demo](https://dev.to/oppnheimer/you-too-can-animate-svg-line-animation-jgm)
- [INKSCAPE](https://inkscape.org/)：SVG 制作工具
- [SVGOMG](https://jakearchibald.github.io/svgomg/): SVG 压缩工具



## 相关库
- [vivus](http://maxwellito.github.io/vivus/)
- [Lazy Line Painter](https://github.com/camoconnell/lazy-line-painter)
- [Walkway](https://github.com/ConnorAtherton/walkway) —— [Demo](https://www.polygon.com/a/ps4-review)

## 参考文章
- [How SVG Line Animation Works](https://css-tricks.com/svg-line-animation-works/)