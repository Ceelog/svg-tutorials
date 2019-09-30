# svg 之 marker

如何使用 SVG 绘制一支 **弓箭** ?

![marker](./imgs/arrow.svg)

 绘制难点在于如何处理箭头和箭羽。要解决这个问题，就得借助 **marker 元素**。

## 定义

**marker 元素**，顾名思义，用于标记 [path](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path), [line](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/line), [polyline](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/polyline) 或 [polygon](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/polygon) 元素的起点，中点或终点的元素。它是一种引用元素。即定义时不会显示，在被引用时才会显示。因此，最好将其定义在 `<defs>` 标签内。

```html
<defs>
  <marker>
  <!-- 定义marker -->
  </marker>
</defs>
```

## 属性

- id

  通过 `"id"` 来引用对应的 marker 元素

  ```html
  <defs id="arrow">
    <marker>
    <!-- 定义marker -->
    </marker>
  </defs>
  ```
- markerWidth 与 markerHeight

  指定 marker 元素的 **viewport** 的宽和高。marker 元素拥有自己的坐标系统。默认值 `markerWidth: 3` , `markerHeight: 3`

  ```html
  <defs >
    <marker markerWidth="30" markerHeight="12">
    <!-- 定义marker -->
    </marker>
  </defs>
  ```

- refX 与 refY

  指定 marker 元素的引用的 x、y 坐标。用于与被引用元素的起点、中点或终点对齐。
  - refX：left|center|right 或 坐标数值。默认值为 0。
  - refY：top|center|bottom 或 坐标数值。默认值为 0。
  

  ```html
  <defs >
    <marker refX="8" refY="0">
    <!-- 定义marker -->
    </marker>
  </defs>
  ```

- orient

- markerUnits
  用于确定marker坐标系统的使用单位
  - strokeWidth：相对于`stroke-width`
  - userSpaceOnUse：绝对单位，设置多少就多少
  
- viewBox 与 preserveAspectRatio

## 引用

- marker-start=”url(#marker-id)”
- marker-mid=”url(#marker-id)”
- marker-end=”url(#marker-id)”
- marker=”url(#marker-id)”
