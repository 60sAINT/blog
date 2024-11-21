---
title: 移动 WEB 开发 - 流式布局
date: 2022-01-31
cover: assets/CSS.png
categories:
- [前端, CSS]
tags:
  - CSS
  - 前端
---

# 移动端基础

## 浏览器现状

**PC 端常见浏览器**

360 浏览器、谷歌浏览器、火狐浏览器、QQ 浏览器、百度浏览器、搜狗浏览器、IE 浏览器。

**移动端常见浏览器**

UC 浏览器，QQ 浏览器，欧朋浏览器，百度手机浏览器，360 安全浏览器，谷歌浏览器，搜狗手机浏览器，猎豹浏览器，以及其他杂牌浏览器。

国内的 UC 和 QQ，百度等手机浏览器都是根据 Webkit 修改过来的内核，国内尚无自主研发的内核，就像国内的手机操作系统都是基于 Android 修改开发的一样。

兼容移动端主流浏览器，处理 Webkit 内核浏览器即可。

## 手机屏幕现状

+   移动端设备屏幕尺寸非常多，碎片化严重。
+   Android 设备有多种分辨率：480x800, 480x854, 540x960, 720x1280，1080x1920 等，还有传说中的 2K，4k 屏。
+   近年来 iPhone 的碎片化也加剧了，其设备的主要分辨率有：640x960, 640x1136, 750x1334, 1242x2208 等。
+   作为开发者无需关注这些分辨率，因为我们常用的尺寸单位是 px。

## 常见移动端屏幕尺寸

数据均参考👉https://material.io/devices

注：作为前端开发，不建议大家去纠结 dp，dpi，pt，ppi 等单位。

## 移动端调试方法

+   Chrome DevTools（谷歌浏览器）的模拟手机调试
+   搭建本地 web 服务器，手机和服务器一个局域网内，通过手机访问服务器
+   使用外网服务器，直接 IP 或域名访问

# 视口

**视口（viewport）** 就是浏览器显示页面内容的屏幕区域。视口可以分为布局视口、视觉视口和理想视口

## 布局视口 layout viewport

+   一般移动设备的浏览器都默认设置了一个布局视口，用于解决早期的 PC 端页面在手机上显示的问题。
+   iOS, Android 基本都将这个视口分辨率设置为 980px，所以 PC 上的网页大多都能在手机上呈现，只不过元素看上去很小，一般默认可以通过手动缩放网页。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230713093634.png)

## 视觉视口 visual viewport

+   字面意思，它是用户正在看到的网站的区域。**注意：是网站的区域。**
+   我们可以通过缩放去操作视觉视口，但不会影响布局视口，布局视口仍保持原来的宽度。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230713100720.png)

## 理想视口 ideal viewport

+   为了使网站在移动端有最理想的浏览和阅读宽度而设定
+   理想视口，对设备来讲，是最理想的视口尺寸
+   需要手动添写 meta 视口标签通知浏览器操作
+   meta 视口标签的主要目的：布局视口的宽度应该与理想视口的宽度一致，（**设备有多宽，我们布局的视口就多宽**）

## meta 视口标签

```html
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

| 属性          | 解释说明                                                 |
| ------------- | -------------------------------------------------------- |
| width         | 宽度设置的是 viewport 宽度，可以设置 device-width 特殊值 |
| initial-scale | 初始缩放比，大于 0 的数字                                |
| maximum-scale | 最大缩放比，大于 0 的数字                                |
| minimum-scale | 最小缩放比，大于 0 的数字                                |
| user-scalable | 用户是否可以缩放，yes 或 no（1 或 0）                    |

## 标准的 viewport 设置

+   视口宽度和设备保持一致
+   视口的默认缩放比例 1.0
+   不允许用户自行缩放
+   最大允许的缩放比例 1.0
+   最小允许的缩放比例 1.0

# 二倍图

## 物理像素 & 物理像素比

+   物理像素点指的是屏幕显示的最小颗粒，是物理真实存在的。这是厂商在出厂时就设置好了，比如苹果 6\\7\\8 是 750\* 1334
    
+   我们开发时候的 1px 不是一定等于 1 个物理像素的
    
+   PC 端页面，1 个 px 等于 1 个物理像素的，但是移动端就不尽相同
    
+   一个 px 的能显示的物理像素点的个数，称为物理像素比或屏幕像素比
    
+   PC 端和早前的手机屏幕 / 普通手机屏幕: 1CSS 像素 = 1 物理像素的
    
+   Retina（视网膜屏幕）是一种显示技术，可以把更多的物理像素点压缩至一块屏幕里，从而达到更高的分辨率，并提高屏幕显示的细腻程度。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230713103617.png)

## 多倍图

+   对于一张 50px \* 50px（css 像素） 的图片，在手机 Retina 屏中打开，按照刚才的物理像素比会放大倍数，这样会造成图片模糊
+   在标准的 viewport 设置中，使用倍图来提高图片质量，解决在高清设备中的模糊问题
+   通常使用二倍图，因为 iPhone 6\\7\\8 的影响，但是现在还存在 3 倍图 4 倍图的情况，这个看实际开发公司需求
+   背景图片注意缩放问题

```html
<body>
    <img src="images/apple50.jpg" alt=""><!-- 模糊的 -->
	<img src="images/apple100.jpg" alt=""><!-- 我们采取 2 倍图 -->
</body>
```

```css
img: nth-child(2) {
    width: 50px;
    height: 50px;
}
/* 我们需要一个 50*50 像素（css 像素）的图片，直接放到我们的 iphone8 里面会放大 2 倍，100*100 就会模糊 */
/* 我们采取的是放一个 100*100 图片，然后手动地把这个图片缩小为 50*50（css 像素）【因为放到移动设备中，要放大 2 倍，所以先缩小为 50*50】 */
/* 我们准备的图片，比我们实际需要的大小大 2 倍，这种方式就是 2 倍图 */
```

# 移动端开发选择

## 单独制作**移动端**页面（主流）

京东商城手机版

淘宝触屏版

苏宁易购手机版....

通常情况下，网址域名前面加 **m (mobile) **可以打开移动端。通过判断设备，如果是移动设备打开，则跳到**移动端页面**。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230714122452.png)

## **响应式**页面兼容移动端（其次）

三星手机官网：www.samsung.com/cn/ ，通过判断屏幕宽度来改变样式，以适应不同终端。

缺点：制作麻烦，需要花很大精力去调兼容性问题

# 移动端技术解决方案

## 移动端浏览器

移动端浏览器基本以 webkit 内核为主，因此我们就考虑 webkit 兼容性问题。

我们可以放心使用 H5 标签和 CSS3 样式。

同时我们浏览器的私有前缀我们只需要考虑添加 webkit 即可

## CSS 初始化 normalize.css

移动端 CSS 初始化推荐使用 normalize.css/

+   保护了有价值的默认值
+   修复了浏览器的 bug
+   是模块化的
+   拥有详细的文档

官网地址：http://necolas.github.io/normalize.css/

## CSS3 盒子模型 box-sizing

+   传统模式宽度计算：盒子的宽度 = CSS 中设置的 width + border + padding
+   CSS3 盒子模型：盒子的宽度 = CSS 中设置的宽度 width 里面包含了 border 和 padding

CSS3 盒子模型：盒子的宽度 = CSS 中设置的宽度 width 里面包含了 border 和 padding

```css
/*CSS3 盒子模型 */
box-sizing: border-box;
/* 传统盒子模型 */
box-sizing: content-box;
```

**传统 or CSS3 盒子模型？**

+   移动端可以全部 CSS3 盒子模型
+   PC 端如果完全需要兼容，我们就用传统模式，如果不考虑兼容性，我们就选择 CSS3 盒子模型

## 特殊样式

```css
/*CSS3 盒子模型 */
box-sizing: border-box;
-webkit-box-sizing: border-box;
/* 点击高亮我们需要清除，设置为 transparent 完成透明 */
-webkit-tap-highlight-color: transparent;
/* 在移动端浏览器默认的外观，在 iOS 上加上这个属性才能给按钮和输入框自定义样式 */
-webkit-appearance: none;
/* 禁用长按页面时的弹出菜单 */
img,a { -webkit-touch-callout: none; }
```

# 流式布局（百分比布局）

+   流式布局，就是百分比布局，也称非固定像素布局。
+   通过盒子的宽度设置成百分比来根据屏幕的**宽度**进行伸缩，不受固定像素的限制，内容向两侧填充。
+   流式布局方式是移动 web 开发使用的比较常见的布局方式。
+   max-width 最大宽度（max-height 最大高度）
+   min-width 最小宽度（min-height 最小高度）

# 案例：京东移动端首页

访问地址：[m.jd.com](https://m.jd.com/)

1. **技术选型**

   方案：我们采取单独制作移动页面方案

   技术：布局采取流式布局

2. **搭建相关文件夹结构**

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230718103711.png)

3. **设置视口标签以及引入初始化样式**

   ```html
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
       <meta http-equiv="X-UA-Compatible" content="ie=edge">
   	<link rel="stylesheet" href="css/normalize.css">
   	<link rel="stylesheet" href="css/index.css">
   </head>
   ```

4. **初始化常用样式**

   ```css
   body {
       margin: 0 auto;
       min-width: 320px;
       max-width: 640px;
       background: #fff;
       font-size: 14px;
       font-family: -apple-system, Helvetica, sans-serif;
       line-height: 1.5;
   }
   
   /* 点击高亮我们需要清除，让所有元素点击完之后都不要高亮显示，设置为 transparent 完成透明 */
   * {
       -webkit-tap-highlight-color: transparent;
   }
   
   /* 在移动端浏览器默认的外观，在 iOS 上加上这个属性才能给按钮和输入框自定义样式 */
   input {
       -webkit-appearance: none;
   }
   
   /* 禁用长按页面时的弹出菜单 */
   img,
   a {
       -webkit-touch-callout: none;
   }
   
   a {
       color: #666;
       text-decoration: none;
   }
   
   img {
       /* 做完 “品牌日” 模块后，发现滑动图和品牌日之间有白色空隙，这是因为滑动图模块的子元素 img 默认与文字基线对齐，导致 img 元素与其父盒子.brand 底部有距离 */
       vertical-align: middle;
   }
   ```

## 头部黑色框

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230717125442.png)

```html
<body>
    <header class="app">
        <ul>
            <li>
                <img src="images/close.png" alt="">
            </li>
            <li>
                <img src="images/logo.png" alt="">
            </li>
            <li>打开京东App，购物更轻松</li>
            <li>立即打开</li>
        </ul>
    </header>
</body>
```

```css
ul {
    margin: 0;
    padding: 0;
    list-style: none;
}

.app {
    height: 45px;
}

.app ul li {
    height: 45px;
    line-height: 45px;
    background-color: #333;
    float: left;
    text-align: center;
    color: #fff;
}

.app ul li:nth-child(1) {
    width: 8%;
}

.app ul li:nth-child(1) img {
    width: 10px;
}

.app ul li:nth-child(2) {
    width: 10%;
}

.app ul li:nth-child(2) img {
    width: 30px;
    /* 给 li 设置 height=line-height 是让 li 中的文字垂直方向居中对齐，而图片的底端默认和文字基线对齐
       由于第 2 个 li 中的图片较高，所以就没有垂直居中的效果了，这时要改变垂直对齐的方式 */
    vertical-align: middle;
}

.app ul li:nth-child(3) {
    width: 57%;
}

.app ul li:nth-child(4) {
    width: 25%;
    background-color: #f63515;
}
```

## 搜索模块

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230717154234.png)

思路：先准备一个父盒子；然后给左右两个子盒子设置绝对定位（脱标）；最后填充一个标准流子盒子，不给宽度，宽度默认为父元素 100%，给这个盒子 margin-left 和 margin-right，就可以实现这种自适应的效果了

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230717130524.png)

```html
<div class="search-wrap">
    <div class="search-btn"></div>
    <div class="search">
        <div class="jd-icon"></div>
        <div class="sou"></div>
    </div>
    <div class="search-login">登陆</div>
</div>
```

```css
.search-wrap {
    position: fixed;
    overflow: hidden;
    width: 100%;
    height: 44px;
    /* 将定位从 relative 改为 fixed 后，该盒子脱标。加了固定的盒子一定要给宽 */
    min-width: 320px;
    max-width: 640px;
}

.search-btn {
    position: absolute;
    top: 0;
    left: 0;
    width: 40px;
    height: 44px;
}

.search-btn::before {
    content: "";
    display: block;
    width: 20px;
    height: 18px;
    background: url(../images/s-btn.png) no-repeat;
    /* 因为背景图片原尺寸太大，所以放在这个伪元素里只能显示出背景图片左上角部分，应该调整背景图片尺寸 */
    background-size: 20px 18px;
    margin: 14px 0 0 15px;
}

.search-login {
    position: absolute;
    top: 0;
    right: 0;
    width: 40px;
    height: 44px;
    color: #fff;
    line-height: 44px;
}

.search {
    position: relative;
    height: 30px;
    margin: 0 50px;
    border-radius: 15px;
    margin-top: 7px;
    /* 给完 margin-top 会发现父盒子有了 7px 上外边距，而子盒子.search 在父盒子内却仍没有上外边距，出现外边距合并问题，解决方法：给父盒子.search-wrap 添加 overflow: hidden; */
    background-color: #fff;
}

.jd-icon {
    width: 20px;
    height: 15px;
    position: absolute;
    top: 8px;
    left: 13px;
    background: url(../images/jd.png) no-repeat;
    background-size: 20px 15px;
}

.jd-icon::after {
    content: "";
    position: absolute;
    right: -8px;
    top: 0;
    display: block;
    width: 1px;
    height: 15px;
    background-color: #ccc;
}

.sou {
    position: absolute;
    top: 8px;
    left: 50px;
    width: 18px;
    height: 15px;
    /* 二倍精灵图做法：
       1. 在 firework 软件里面把精灵图等比例缩放为原来的一半（注意不要在 firework 里保存修改，否则就修改原图了）
       2. 再测量坐标
       3. 注意代码里面 background-size 也要写：精灵图原来宽度的一半 */
    background: url(../images/jd-sprites.png) no-repeat -81px 0;
    background-size: 200px auto;
}
```

## 主体内容部分

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230718103417.png)

```html
<div class="main-content">
        <!-- 滑动图 -->
        <div class="slider">
            <img src="upload/banner.dpg" alt="">
        </div>
        <!-- 小家电品牌日 -->
        <div class="brand">
            <div>
                <a href="#"><img src="upload/pic11.dpg" alt=""></a>
            </div>
            <div>
                <a href="#"><img src="upload/pic22.dpg" alt=""></a>
            </div>
            <div>
                <a href="#"><img src="upload/pic33.dpg" alt=""></a>
            </div>
        </div>
        <!-- nav 部分 -->
        <nav>
            <a href="">
                <img src="upload/nav1.webp" alt="">
                <span>京东超市</span>
            </a>
            <a href="">
                <img src="upload/nav2.webp" alt="">
                <span>京东超市</span>
            </a>
            <a href="">
                <img src="upload/nav1.webp" alt="">
                <span>京东超市</span>
            </a>
            <a href="">
                <img src="upload/nav1.webp" alt="">
                <span>京东超市</span>
            </a>
            <a href="">
                <img src="upload/nav1.webp" alt="">
                <span>京东超市</span>
            </a>
            <a href="">
                <img src="upload/nav3.webp" alt="">
                <span>京东超市</span>
            </a>
            <a href="">
                <img src="upload/nav1.webp" alt="">
                <span>京东超市</span>
            </a>
            <a href="">
                <img src="upload/nav1.webp" alt="">
                <span>京东超市</span>
            </a>
            <a href="">
                <img src="upload/nav1.webp" alt="">
                <span>京东超市</span>
            </a>
            <a href="">
                <img src="upload/nav1.webp" alt="">
                <span>京东超市</span>
            </a>
        </nav>
        <!-- 新闻模块 -->
        <div class="news">
            <a href="#">
                <img src="upload/new1.dpg" alt="">
            </a>
            <a href="#">
                <img src="upload/new2.dpg" alt="">
            </a>
            <a href="#">
                <img src="upload/new3.dpg" alt="">
            </a>
        </div>
</div>
```

```css
.slider img {
    width: 100%;
}

/* 小家电品牌日 */
.brand {
    border-radius: 10px 10px 0 0;
    /* 给父盒子加圆角后发现没效果，这是因为图片溢出挡住了圆角 */
    overflow: hidden;
}

.brand div {
    float: left;
    width: 33.33%;
}

.brand div img {
    width: 100%;
}

/* nav */
nav {
    padding-top: 5px;
}

nav a {
    float: left;
    width: 20%;
    text-align: center;
}

nav a img {
    width: 40px;
    margin: 10px 0;
}

nav a span {
    display: block;
}

/* news */
.news {
    margin-top: 20px;
}

.news a {
    float: left;
    box-sizing: border-box;
}

.news a:nth-child(1) {
    width: 50%;
}

.news a:nth-child(n+2) {
    width: 25%;
    border-left: 1px solid #ccc;
    /* 加上边框后发现第三个 a 标签掉下来了，解决方法：设置每个 a 标签为 border-box，这样 border 值就算进盒子大小了，增加边框不会撑大盒子 */
}

.news img {
    width: 100%;
}
```

