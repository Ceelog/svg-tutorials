# svg 分组与复用

在讲解之前，先思考一个问题："svg 为什么需要 **“分组”**，**“分组”** 能解决什么问题❓"

```html
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="xMidYMid meet" viewBox="0 0 640 300" width="640" height="300">
    <path d="M226.07 41.67L109.14 85L89.14 215L209.14 236.67L189.14 280L69.14 236.67L85.41 63.33L209.14 20L226.07 41.67Z"></path>
    <path d="M370.45 63.96L342.11 40L260.8 60L286.67 127.69L402.48 180L326.1 280L219.41 260L241.83 237.58L328.56 262.86L376.36 180L264.25 140L238.63 60L353.94 20L376.36 40L370.45 63.96Z"></path>
    <path d="M538.82 63.96L510.49 40L429.18 60L455.05 127.69L570.86 180L494.47 280L387.78 260L410.2 237.58L496.94 262.86L544.74 180L432.63 140L407 60L522.32 20L544.74 40L538.82 63.96Z"></path>
</svg>
```

![svg](./imgs/CSS-INIT.svg)

如上图 `CSS 字样` 是一个整体，如果想要修改它的颜色，需要设置每个 `<path>` 的 `"fill"` 属性。

```html
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="xMidYMid meet" viewBox="0 0 640 300" width="640" height="300">
    <path fill="#4CAF50" d="M226.07 41.67L109.14 85L89.14 215L209.14 236.67L189.14 280L69.14 236.67L85.41 63.33L209.14 20L226.07 41.67Z"></path>
    <path fill="#4CAF50" d="M370.45 63.96L342.11 40L260.8 60L286.67 127.69L402.48 180L326.1 280L219.41 260L241.83 237.58L328.56 262.86L376.36 180L264.25 140L238.63 60L353.94 20L376.36 40L370.45 63.96Z"></path>
    <path fill="#4CAF50" d="M538.82 63.96L510.49 40L429.18 60L455.05 127.69L570.86 180L494.47 280L387.78 260L410.2 237.58L496.94 262.86L544.74 180L432.63 140L407 60L522.32 20L544.74 40L538.82 63.96Z"></path>
</svg>
```

![svg](./imgs/CSS-COLOR.svg)

现在只修改 `"fill"`，如果要整体移动，或动画，还需要对每个 `<path>` 分别做计算。但如果将其组成一个整体，事情就变得简单啦！

## `<g>`

```html
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="xMidYMid meet" viewBox="0 0 640 300" width="640" height="300">
    <g fill="#4CAF50">
        <path d="M226.07 41.67L109.14 85L89.14 215L209.14 236.67L189.14 280L69.14 236.67L85.41 63.33L209.14 20L226.07 41.67Z"></path>
        <path d="M370.45 63.96L342.11 40L260.8 60L286.67 127.69L402.48 180L326.1 280L219.41 260L241.83 237.58L328.56 262.86L376.36 180L264.25 140L238.63 60L353.94 20L376.36 40L370.45 63.96Z"></path>
        <path d="M538.82 63.96L510.49 40L429.18 60L455.05 127.69L570.86 180L494.47 280L387.78 260L410.2 237.58L496.94 262.86L544.74 180L432.63 140L407 60L522.32 20L544.74 40L538.82 63.96Z"></path>
    </g>
</svg>
```

![svg](./imgs/CSS-GROUP.svg)

通过 `<g></g>` 对每个 `<path>` 进行包裹，组成为一个整体（即让 `<g>` 成为所有的 `<path>` 父节点）。再设置 `<g>` 的 `fill="#4CAF50"`，这样每个 `<path>` 都会继承 `<g>` 的 `fill="#4CAF50"`。当然如果 `<path>` 自己设置了 `fill`， 会覆盖继承的 `fill`。类似于 HTML 节点元素的继承特性。


```html
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="xMidYMid meet" viewBox="0 0 640 300" width="640" height="300">
    <g fill="#4CAF50" stroke="#0c2d0f" stroke-width="6" style="transform: scale(0.8) rotate(180deg) translate(-100%, -100%);">
        <path d="M226.07 41.67L109.14 85L89.14 215L209.14 236.67L189.14 280L69.14 236.67L85.41 63.33L209.14 20L226.07 41.67Z"></path>
        <path d="M370.45 63.96L342.11 40L260.8 60L286.67 127.69L402.48 180L326.1 280L219.41 260L241.83 237.58L328.56 262.86L376.36 180L264.25 140L238.63 60L353.94 20L376.36 40L370.45 63.96Z"></path>
        <path d="M538.82 63.96L510.49 40L429.18 60L455.05 127.69L570.86 180L494.47 280L387.78 260L410.2 237.58L496.94 262.86L544.74 180L432.63 140L407 60L522.32 20L544.74 40L538.82 63.96Z"></path>
    </g>
</svg>

```

![svg](./imgs/CSS-GROUP-MORE.svg)

**“分组”** 除了可以整体控制状态外，还能方便 **“复用”**。

## `<use>`

要 **“复用”** 已定义好的图形，可以使用 `<use>` 进行引用。

```html
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="xMidYMid meet" viewBox="0 0 640 600" width="640" height="300">
    <g id="v" >
        <path d="M226.07 41.67L109.14 85L89.14 215L209.14 236.67L189.14 280L69.14 236.67L85.41 63.33L209.14 20L226.07 41.67Z"></path>
        <path d="M370.45 63.96L342.11 40L260.8 60L286.67 127.69L402.48 180L326.1 280L219.41 260L241.83 237.58L328.56 262.86L376.36 180L264.25 140L238.63 60L353.94 20L376.36 40L370.45 63.96Z"></path>
        <path d="M538.82 63.96L510.49 40L429.18 60L455.05 127.69L570.86 180L494.47 280L387.78 260L410.2 237.58L496.94 262.86L544.74 180L432.63 140L407 60L522.32 20L544.74 40L538.82 63.96Z"></path>
    </g>
    <use xlink:href="#v" x="0" y="60"  fill="#4CAF50" stroke="#0c2d0f" stroke-width="6" style="transform: scale(0.6) rotate(180deg) translate(-20%, -170%);"/>
    <use xlink:href="#v" x="0" y="60"  fill="#4CAF50" stroke="#0c2d0f" stroke-width="6" style="transform: scale(0.4) rotate(180deg) translate(-150%, -240%);"/>
    <use xlink:href="#v" x="0" y="60"  fill="#4CAF50" stroke="#0c2d0f" stroke-width="6" style="transform: scale(0.2) rotate(180deg) translate(-460%, -460%);"/>
</svg>
```
要引用已定义的图形，需要对已定义图形设置一个 `id`。 在通过 `<use xlink:href="#id" />` 进行引用。同时还可以对引用图形进行位置、样式、动画等定制。

![svg](./imgs/CSS-GROUP-MORE-USE.svg)

⚠️ 但现在存在一个问题，引用只能对已定义并显示的图形。这种 **"复用"** 方式不是很好，更希望定义的图形不显示，只有在引用时才会显示。要实现这种 **"复用"** 方式，可以使用 `<defs>`。

## `<defs>`

`<g>` 与 `<defs>` 最大的不同的在于 `<defs>` 内定义的内容不会显示。只要在通过`<use>` 引用时才会显示。

```html
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="xMidYMid meet" viewBox="0 0 640 600" width="640" height="300">
    <defs>
        <g id="v" >
            <path d="M226.07 41.67L109.14 85L89.14 215L209.14 236.67L189.14 280L69.14 236.67L85.41 63.33L209.14 20L226.07 41.67Z"></path>
            <path d="M370.45 63.96L342.11 40L260.8 60L286.67 127.69L402.48 180L326.1 280L219.41 260L241.83 237.58L328.56 262.86L376.36 180L264.25 140L238.63 60L353.94 20L376.36 40L370.45 63.96Z"></path>
            <path d="M538.82 63.96L510.49 40L429.18 60L455.05 127.69L570.86 180L494.47 280L387.78 260L410.2 237.58L496.94 262.86L544.74 180L432.63 140L407 60L522.32 20L544.74 40L538.82 63.96Z"></path>
        </g>
    </defs>
    <use xlink:href="#v" x="0" y="60" fill="#E91E63" />
    <use xlink:href="#v" x="0" y="60"  fill="#4CAF50" stroke="#0c2d0f" stroke-width="6" style="transform: scale(0.6) rotate(180deg) translate(-20%, -170%);"/>
    <use xlink:href="#v" x="0" y="60"  fill="#4CAF50" stroke="#0c2d0f" stroke-width="6" style="transform: scale(0.4) rotate(180deg) translate(-150%, -240%);"/>
    <use xlink:href="#v" x="0" y="60"  fill="#4CAF50" stroke="#0c2d0f" stroke-width="6" style="transform: scale(0.2) rotate(180deg) translate(-460%, -460%);"/>
</svg>
```

![svg](./imgs/CSS-GROUP-MORE-DEFS.svg)

现在还存在一个问题，如果只想显示引用图形的某个部分（即控制引用图形的具体显示内容），`<defs>` 就无能为力了，此时只能借助 `<symbol>` 了。

## `<symbol>`

`<symbol>` 除了拥有 `<defs>` 不显示内容特性外，还有自己的 `viewBox` 及 `preserveAspectRatio` 特性，即有自己的坐标系统。

通过 `<symbol>` 的 `viewBox` 及 `preserveAspectRatio` 只显示图形 `CSS 字样` 中的 `C 字样`

```html
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" preserveAspectRatio="xMidYMid meet" viewBox="0 0 640 600" width="640" height="300">
    <symbol id="v" viewBox="0 0 3 400">
        <g>
            <path d="M226.07 41.67L109.14 85L89.14 215L209.14 236.67L189.14 280L69.14 236.67L85.41 63.33L209.14 20L226.07 41.67Z"></path>
            <path d="M370.45 63.96L342.11 40L260.8 60L286.67 127.69L402.48 180L326.1 280L219.41 260L241.83 237.58L328.56 262.86L376.36 180L264.25 140L238.63 60L353.94 20L376.36 40L370.45 63.96Z"></path>
            <path d="M538.82 63.96L510.49 40L429.18 60L455.05 127.69L570.86 180L494.47 280L387.78 260L410.2 237.58L496.94 262.86L544.74 180L432.63 140L407 60L522.32 20L544.74 40L538.82 63.96Z"></path>
        </g>
    </symbol>
    <use xlink:href="#v" x="-340" y="0" fill="#E91E63" />
    <use xlink:href="#v" x="0" y="60"  fill="#4CAF50" stroke="#0c2d0f" stroke-width="6" style="transform: scale(0.6) rotate(180deg) translate(-20%, -180%);"/>
    <use xlink:href="#v" x="0" y="60"  fill="#4CAF50" stroke="#0c2d0f" stroke-width="6" style="transform: scale(0.4) rotate(180deg) translate(-150%, -250%);"/>
    <use xlink:href="#v" x="0" y="60"  fill="#4CAF50" stroke="#0c2d0f" stroke-width="6" style="transform: scale(0.2) rotate(180deg) translate(-460%, -470%);"/>
</svg>

```

![svg](./imgs/CSS-GROUP-MORE-SYMBOL.svg)

