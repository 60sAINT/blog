---
title: 移动 WEB 开发 - flex 布局
date: 2022-02-02
cover: assets/CSS.png
categories:
- [前端, CSS]
tags:
  - CSS
  - 前端
---

# flex 布局体验

**传统布局**

+   兼容性好
+   布局繁琐
+   局限性，不能在移动端很好的布局

**flex 弹性布局**

+   操作方便，布局极为简单，移动端应用很广泛
+   PC 端浏览器支持情况较差

建议：

1.  如果是 PC 端页面布局，我们还是传统布局。
    
2.  如果是移动端或者不考虑兼容性问题的 PC 端页面布局，我们还是使用 flex 弹性布局
    
3.  **搭建 HTML 结构**
    
    ```html
    <div>
        <span>1</span>
        <span>2</span>
        <span>3</span>
    </div>
    ```
    
    +   里面的 3 个 span 是行内元素
4.  **CSS 样式**
    
    1.  span 直接给宽度和高度，背景颜色，还有蓝色边框
    2.  给 div 只需要添加 “display: flex” 即可

# flex 布局原理

flex 是 flexible Box 的缩写，意为 "弹性布局"，用来为盒状模型提供最大的灵活性，任何一个容器都可以指定为 flex 布局。

+   当我们为父盒子设为 flex 布局以后，子元素的 float、clear 和 vertical-align 属性将失效。
+   伸缩布局 = 弹性布局 = 伸缩盒布局 = 弹性盒布局 = flex 布局

采用 Flex 布局的元素，称为 Flex 容器（flex container），简称 "容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称 "项目"。

+   体验中 div 就是 flex 父容器。
+   体验中 span 就是子容器 flex 项目
+   子容器可以横向排列也可以纵向排列

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230714131516.png)

# flex 布局父项常见属性

## flex-direction 设置主轴方向

1.  **主轴与侧轴**
    
    在 flex 布局中，是分为主轴和侧轴两个方向，同样的叫法有：行和列、x 轴和 y 轴
    
    +   默认主轴方向就是 x 轴方向，水平向右
    +   默认侧轴方向就是 y 轴方向，水平向下
2.  **属性值**
    
    flex-direction 属性决定主轴的方向（即项目的排列方向）
    
    注意：主轴和侧轴是会变化的，就看 flex-direction 设置谁为主轴，剩下的就是侧轴。而我们的子元素是跟着主轴来排列的
    
    | 属性值         | 说明                                                         |
    | -------------- | ------------------------------------------------------------ |
    | row            | 默认值从左到右                                               |
    | row-reverse    | 从右到左（flex 项目从右侧开始对齐，页面排列顺序与代码顺序相反） |
    | column         | 从上到下                                                     |
    | column-reverse | 从下到上                                                     |
    

## justify-content 设置主轴上的子元素排列方式

justify-content 属性定义了项目在主轴上的对齐方式

注意：使用这个属性之前一定要确定好主轴是哪个

| 属性值        | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| flex-start    | 默认值，从头部开始。如果主轴是 x 轴，则从左到右              |
| flex-end      | 从尾部开始排列（flex 项目右侧对齐，页面排列顺序仍与代码顺序相同） |
| center        | 在主轴居中对齐（如果主轴是 x 轴则水平居中）                  |
| space-around  | 平分剩余空间                                                 |
| space-between | 先两边贴边，再平分剩余空间                                   |

## flex-wrap 设置子元素是否换行

默认情况下，项目都排在一条线（又称” 轴线”）上。flex-wrap 属性定义，flex 布局中默认是不换行的。

| 属性值 | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| nowrap | 默认值，不换行。如果 flex 项目显示不开，就缩小 flex 项目元素的大小 |
| wrap   | 换行                                                         |

## flex-flow

flex-flow 属性是 flex-direction 和 flex-wrap 属性的复合属性

```css
flex-flow: row wrap;
```

## align-items 设置侧轴上的子元素排列方式（单行）

该属性是控制子项在侧轴（默认是 y 轴）上的排列方式 在子项为单项（单行）的时候使用

| 属性值     | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| flex-start | 从上到下                                                     |
| flex-end   | 从下到上                                                     |
| center     | 挤在一起居中（垂直居中）                                     |
| stretch    | 拉伸（默认值 ），子盒子高度等于父项高度。但是子盒子不要给高度，若给了则显示 flex-start 的效果 |

## align-content 设置侧轴上的子元素的排列方式（多行）

设置子项在侧轴上的排列方式并且只能用于子项出现换行的情况（多行），在单行下是没有效果的。

| 属性值        | 说明                                   |
| ------------- | -------------------------------------- |
| flex-start    | 默认值，在侧轴的头部开始排列           |
| flex-end      | 在侧轴的尾部开始排列                   |
| center        | 在侧轴中间显示                         |
| space-around  | 子项在侧轴平分剩余空间                 |
| space-between | 子项在侧轴先分布在两头，再平分剩余空间 |
| stretch       | 设置子项元素高度平分父元素高度         |

# flex 布局子项常见属性

## flex 属性

flex 属性定义子项目分配剩余空间，用 flex 来表示占多少份数。

```css
.item {
    flex: <number>; /* default 0 */
}
```

```html
<section>
    <div></div>
    <div></div>
    <div></div>
</section>
```

```css
section {
    display: flex;
    width: 60%;
    height: 150px;
    margin: 0 auto;
}
section div:nth-child(1) {
    width: 100px;
    height: 150px;
}
section div:nth-child(2) {
    flex: 1;
}
section div:nth-child(1) {
    width: 100px;
    height: 150px;
}
```

## align-self 控制子项自己在侧轴上的排列方式

align-self 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性。

默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch。

```css
span:nth-child(2) {
    /* 只让 2 号盒子下来底侧 */
    align-self: flex-end;
}
```

## order 属性定义项目的排列顺序

数值（可以为负值）越小，排列越靠前，默认为 0。

注意：和 z-index 不一样。

```css
.item {
    order: <number>;
}
```

# 案例：携程网移动端首页

访问地址：[m.ctrip.com](https://m.ctrip.com/html5/)

1. **技术选型**

   方案：我们采取单独制作移动页面方案

   技术：布局采取 flex 布局

2. **搭建相关文件夹结构**

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230718105719.png)

3. **设置视口标签以及引入初始化样式**

   ```html
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, user-scalable=no,
       initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
       <link rel="stylesheet" href="css/normalize.css">
       <link rel="stylesheet" href="css/index.css">
   </head>
   ```

4. **常用初始化样式**

   ```css
   body {
       max-width: 540px;
       min-width: 320px;
       margin: 0 auto;
       font: normal 14px/1.5 Tahoma, "Lucida Grande", Verdana, " Microsoft Yahei", STXihei, hei;
       color: #000;
       background: #f2f2f2;
       /* 永远不会出现水平滚动条 */
       overflow-x: hidden;
       -webkit-tap-highlight-color: transparent;
   }
   
   ul {
       list-style: none;
       margin: 0;
       padding: 0;
   }
   
   a {
       text-decoration: none;
       color: #222;
   }
   
   div {
       box-sizing: border-box;
   }
   ```

## 顶部搜索模块

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230718160711.png)

```html
<div class="search-index">
	<div class="search">搜索：目的地/酒店/景点/航班号</div>
	<a href="" class="user">我 的</a>
</div>
```

```css
/* 搜索模块 */
.search-index {
    display: flex;
    position: fixed;
    top: 0;
    left: 50%;
    /* 兼容老版本谷歌浏览器 */
    -webkit-transform: translateX(-50%);
    transform: translateX(-50%);
    /* 不写上面两行，盒子也是水平居中对齐 */
    /* 固定的盒子必须有宽度 */
    width: 100%;
    /* 因为固定定位盒子的宽度与父元素无关，以屏幕为准，所以要设定最大宽度，避免盒子宽度铺满屏幕 */
    min-width: 320px;
    max-width: 540px;
    height: 44px;
    background-color: #f6f6f6;
    border-top: 1px solid #ccc;
    border-bottom: 1px solid #ccc;
}

.search {
    position: relative;
    height: 26px;
    /* 让文字在盒子中垂直居中 = 文字在盒子内容区垂直居中，对于传统盒模型内容区高度 = height，
       而对于 border-box 来说内容区 = height-2*border，这样计算得到 line-hight = 盒子内容区高度 */
    line-height: 24px;
    border: 1px solid #ccc;
    flex: 1;
    font-size: 12px;
    color: #666;
    margin: 7px 10px;
    padding-left: 25px;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, .2);
}

.search::before {
    content: "";
    position: absolute;
    top: 5px;
    left: 5px;
    width: 15px;
    height: 15px;
    background: url(../images/sprite.png) no-repeat -59px -279px;
    background-size: 104px auto;
}

.user {
    width: 44px;
    height: 44px;
    font-size: 12px;
    text-align: center;
    color: #2eaae0;
}

.user::before {
    content: "";
    display: block;
    width: 23px;
    height: 23px;
    background: url(../images/sprite.png) no-repeat -59px -194px;
    background-size: 104px auto;
    margin: 4px auto -2px;
}
```

## 焦点图 focus 模块

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230720101856.png)

```html
<div class="focus">
        <img src="upload/focus.jpg" alt="">
</div>
```

```css
.focus {
    padding-top: 44px;
}

.focus img {
    width: 100%;
}
```

## 局部导航栏模块

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230720105824.png)

```html
<ul class="local-nav">
        <li>
            <a href="#" title="景点·玩乐">
                <span class="local-nav-icon-icon1"></span>
                <span>景点·玩乐</span>
            </a>
        </li>
        <li>
            <a href="#" title="景点·玩乐">
                <span class="local-nav-icon-icon2"></span>
                <span>景点·玩乐</span>
            </a>
        </li>
        <li>
            <a href="#" title="景点·玩乐">
                <span class="local-nav-icon-icon3"></span>
                <span>景点·玩乐</span>
            </a>
        </li>
        <li>
            <a href="#" title="景点·玩乐">
                <span class="local-nav-icon-icon4"></span>
                <span>景点·玩乐</span>
            </a>
        </li>
        <li>
            <a href="#" title="景点·玩乐">
                <span class="local-nav-icon-icon5"></span>
                <span>景点·玩乐</span>
            </a>
        </li>
</ul>
```

```css
.local-nav {
    display: flex;
    height: 64px;
    background-color: #fff;
    border-radius: 8px;
    margin: 3px 4px;
}

.local-nav li {
    flex: 1;
}

/* 让每个 li（中的 a 标签）作为 flex 容器，上面的图标和下面的文字为 2 个 flex 项目，主轴为 y 轴 */
.local-nav a {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-size: 12px;
}

/* 利用属性选择器更换背景图片 */
.local-nav li [class^="local-nav-icon"] {
    width: 32px;
    height: 32px;
    margin-top: 8px;
    background: url(../images/localnav_bg.png) no-repeat 0 0;
    background-size: 32px auto;
}

.local-nav li .local-nav-icon-icon2 {
    background-position: 0 -32px;
}

.local-nav li .local-nav-icon-icon3 {
    background-position: 0 -64px;
}

.local-nav li .local-nav-icon-icon4 {
    background-position: 0 -96px;
}

.local-nav li .local-nav-icon-icon5 {
    background-position: 0 -128px;
}
```

## nav 模块

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230720124937.png)

```html
<nav>
        <div class="nav-common">
            <div class="nav-items">
                <a href="#">海外酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
        </div>
        <div class="nav-common">
            <div class="nav-items">
                <a href="#">海外酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
        </div>
        <div class="nav-common">
            <div class="nav-items">
                <a href="#">海外酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
            <div class="nav-items">
                <a href="#">海外酒店</a>
                <a href="#">特价酒店</a>
            </div>
        </div>
</nav>
```

```css
nav {
    border-radius: 8px;
    /* 避免 nav 中的上下两个 div 把圆角遮住 */
    overflow: hidden;
    margin: 0 4px 3px;
}

.nav-common {
    height: 88px;
    display: flex;
}

.nav-common:nth-child(2) {
    margin: 3px 0;
}

.nav-items {
    flex: 1;
    display: flex;
    flex-direction: column;
}

.nav-items a {
    flex: 1;
    text-align: center;
    line-height: 44px;
    color: #fff;
    font-size: 14px;
    text-shadow: 1px 1px rgba(0, 0, 0, .2);
}

.nav-items:nth-child(-n+2) {
    border-right: 1px solid #fff;
}

.nav-items a:nth-child(1) {
    border-bottom: 1px solid #fff;
}

.nav-items:nth-child(1) a {
    border: 0;
    background: url(../images/hotel.png) no-repeat bottom center;
    background-size: 121px;
}

.nav-common:nth-child(1) {
    background: -webkit-linear-gradient(left, #fa5a55, #fa994d);
}

.nav-common:nth-child(2) {
    background: -webkit-linear-gradient(left, #4b90ed, #53bced);
}

.nav-common:nth-child(3) {
    background: -webkit-linear-gradient(left, #34c2a9, #6cd559);
}
```

## subnav-entry 模块

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230721151555.png)

```html
<ul class="subnav-entry">
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
        <li>
            <a href="#">
                <span class="subnav-entry-icon"></span>
                <span>电话费</span>
            </a>
        </li>
</ul>
```

```css
.subnav-entry {
    border-radius: 8px;
    background-color: #fff;
    margin: 0 4px;
    display: flex;
    flex-wrap: wrap;
    padding: 5px 0;
}

.subnav-entry li {
    flex: 20%;/* 相对于父级 */
}

.subnav-entry a {
    display: flex;
    flex-direction: column;
    /* 图标和文字水平居中，即设置侧轴上的子元素排列方式（单行） */
    align-items: center;
}
.subnav-entry-icon {
    width: 28px;
    height: 28px;
    margin-top: 4px;
    background: url(../images/subnav-bg.png) no-repeat;
    background-size: 28px auto;
}
```

## 销售模块

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230724100450.png)

```html
<div class="sales-box">
        <div class="sales-hd">
            <!-- 设置 h2 不是为了显示文字 “热门活动”，而是把他当作相对定位的父盒子，内部添加伪元素放绝对定位的图片 -->
            <h2>热门活动</h2>
            <a href="" class="more">获取更多福利</a>
        </div>
        <div class="sales-bd">
            <div class="row">
                <a href="#">
                    <img src="upload/pic1.jpg" alt="">
                </a>
                <a href="#">
                    <img src="upload/pic2.jpg" alt="">
                </a>
            </div>
            <div class="row">
                <a href="#">
                    <img src="upload/pic3.jpg" alt="">
                </a>
                <a href="#">
                    <img src="upload/pic4.jpg" alt="">
                </a>
            </div>
            <div class="row">
                <a href="#">
                    <img src="upload/pic5.jpg" alt="">
                </a>
                <a href="#">
                    <img src="upload/pic6.jpg" alt="">
                </a>
            </div>
        </div>
</div>
```

```css
.sales-box {
    border-top: 1px solid #ccc;
    background-color: #fff;
    margin: 4px;
}

.sales-hd {
    height: 44px;
    border-bottom: 1px solid #ccc;
    position: relative;
}

.sales-hd h2 {
    /* text-indent 指定文本第一行的缩进 */
    text-indent: -999px;
    overflow: hidden;
    position: relative;
}

.sales-hd h2::after {
    position: absolute;
    top: 8px;
    left: 20px;
    content: "";
    width: 79px;
    height: 15px;
    background: url(../images/hot.png) no-repeat 0 -20px;
    background-size: 79px auto;
}

.more {
    position: absolute;
    right: 5px;
    top: 0px;
    background: -webkit-linear-gradient(left, #ff506c, #ff6bc6);
    border-radius: 15px;
    padding: 3px 20px 3px 10px;
    color: #fff;
}

.more::after {
    content: "";
    position: absolute;
    top: 9px;
    right: 9px;
    width: 7px;
    height: 7px;
    border-top: 2px solid #fff;
    border-left: 2px solid #fff;
    transform: rotate(135deg);
}

.row {
    display: flex;
}

.row a {
    flex: 1;
    border-bottom: 1px solid #ccc;
}

.row a:nth-child(1) {
    border-right: 1px solid #ccc;
}

.row a img {
    width: 100%;
}
```

