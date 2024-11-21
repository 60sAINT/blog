---
title: 移动 WEB 开发 - rem 布局
date: 2022-02-03
cover: assets/CSS.png
categories:
- [前端, CSS]
tags:
  - CSS
  - 前端
---

# rem 单位

rem (root em) 是一个相对单位，类似于 em，em 是父元素字体大小。

不同的是 rem 的基准是相对于 html 元素的字体大小。

比如，根元素（html）设置 font-size=12px; 非根元素设置 width: 2rem; 则换成 px 表示就是 24px。

rem 的优势：父元素文字大小可能不一致，但是整个页面只有一个 html，可以很好来控制整个页面的元素大小

```css
/* 根 html 为 12px */
html {
	font-size: 12px;
}
/* 此时 div 的字体大小就是 24px */
div {
	font-size: 2rem;
}
```

# 媒体查询

## 什么是媒体查询

媒体查询（**Media Query**）是 CSS3 新语法。

+   使用 @media 查询，可以针对不同的媒体类型定义不同的样式
+   @media 可以针对不同的屏幕尺寸设置不同的样式
+   当你重置浏览器大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面
+   目前针对很多苹果手机、Android 手机，平板等设备都用得到多媒体查询

## 语法规范

```css
@media mediatype and|not|only (media feature) {
	CSS-Code;
}
```

+   用 @media 开头注意 @符号
+   mediatype 媒体类型
+   关键字 and not only
+   media feature 媒体特性，必须有小括号包含

1.  **mediatype 查询类型**
    
    将不同的终端设备划分成不同的类型，称为媒体类型
    
    | 值     | 解释说明                           |
    | ------ | ---------------------------------- |
    | all    | 用于所有设备                       |
    | print  | 用于打印机和打印预览               |
    | screen | 用于电脑屏幕，平板电脑，智能手机等 |
    
2.  **关键字**
    
    关键字将媒体类型或多个媒体特性连接到一起作为媒体查询的条件。
    
    +   and：可以将多个媒体特性连接到一起，相当于 “且” 的意思。
    +   not：排除某个媒体类型，相当于 “非” 的意思，可以省略。
    +   only：指定某个特定的媒体类型，可以省略。
3.  **媒体特性**
    
    每种媒体类型都具有各自不同的特性，根据不同媒体类型的媒体特性设置不同的展示风格。我们暂且了解三个。注意他们要加小括号包含
    
    | 值        | 解释说明                           |
    | --------- | ---------------------------------- |
    | width     | 定义输出设备中页面可见区域的宽度   |
    | min-width | 定义输出设备中页面最小可见区域宽度 |
    | max-width | 定义输出设备中页面最大可见区域宽度 |
    

```css
@media screen and (max-width: 800px) {
    body {
        background-color: pink;
    }
}
@media screen and (max-width: 500px) {
    body {
        background-color: purple;
    }
}
/* 屏幕宽度为 0-500px 时背景为紫色，500-800px 时背景为粉色 */
```

## 媒体查询 + rem 实现元素动态大小变化

rem 单位是跟着 html 来走的，有了 rem 页面元素可以设置不同大小尺寸

媒体查询可以根据不同设备宽度来修改样式

媒体查询 + rem 就可以实现不同设备宽度，实现页面元素大小的动态变化

```html
<div class="top">购物车</div>
```

```css
@media screen and (min-width: 320px) {
    html {
        font-size: 50px;
    }
}
@media screen and (min-width: 640px) {
    html {
        font-size: 100px;
    }
}
.top {
    height: 1rem;
    font-size: .5rem;
    line-height: 1rem;
}
```

## 引入资源

当样式比较繁多的时候，我们可以针对不同的媒体使用不同 stylesheets（样式表）。

原理，就是直接在 link 中判断设备的尺寸，然后引用不同的 css 文件。

1.  **语法规范**
    
    ```html
    <link rel="stylesheet" media="mediatype and|not|only (media feature)" href="mystylesheet.css">
    ```
    
2.  **示例**
    
    ```html
    <link rel="stylesheet" href="styleA.css" media="screen and (min-width: 400px)">
    ```

# rem 适配方案

1.  按照设计稿与设备宽度的比例，动态计算并设置 html 根标签的 font-size 大小；（媒体查询）
2.  CSS 中，设计稿元素的宽、高、相对位置等取值，按照同等比例换算为 rem 为单位的值；

## 技术方案 1

+   less
+   媒体查询
+   rem

1.  **设计稿常见尺寸宽度**
    
    | 设备                              | 常见宽度                                                    |
    | --------------------------------- | ----------------------------------------------------------- |
    | iphone 4.5                        | 640px                                                       |
    | iphone 678                        | 750px                                                       |
    | Android                           | 常见 320px、360px、375px、384px、400px、414px、500px、720px |
    | 大部分 4.7~5 寸的安卓设备为 720px |                                                             |
    
    一般情况下，我们以一套或两套效果图适应大部分的屏幕，放弃极端屏或对其优雅降级，牺牲一些效果
    
    现在基本以 750 为准。
    
2.  **动态设置 html 标签 font-size 大小**
    
3.  假设设计稿是 750px
    
4.  假设我们把整个屏幕划分为 15 等份（划分标准不一可以是 20 份也可以是 10 等份）
    
5.  每一份作为 html 字体大小，这里就是 50px
    
6.  那么在 320px 设备的时候，字体大小为 320/15 就是 21.33px
    
7.  用我们页面元素的大小除以不同的 html 字体大小会发现他们比例还是相同的
    
8.  比如我们以 750 为标准设计稿
    
9.  一个 100\*100 像素的页面元素在 750 屏幕下，就是 100 / 50 转换为 rem 是 2rem \*2 rem 比例是 1 比 1
    
10.  320 屏幕下，html 字体大小为 21.33，则 2rem = 42.66px 此时宽和高都是 42.66 但是宽和高的比例还是 1 比 1
    
11.  但是已经能实现不同屏幕下 页面元素盒子等比例缩放的效果
    
12.  **元素大小取值方法**
     
    1.  最后的公式：页面元素的 rem 值 = 页面元素值（px）/（屏幕宽度 / 划分的份数）
    2.  屏幕宽度 / 划分的份数就是 html font-size 的大小
    3.  或者：页面元素的 rem 值 = 页面元素值（px）/html font-size 字体大小

## 技术方案 2（推荐）

+   flexible.js
+   rem

手机淘宝团队出的简洁高效移动端适配库

我们再也不需要在写不同屏幕的媒体查询，因为里面 js 做了处理

它的原理是把当前设备划分为 10 等份，但是不同设备下，比例还是一致的。

我们要做的，就是确定好我们当前设备的 html 文字大小就可以了

比如当前设计稿是 750px，那么我们只需要把 html 文字大小设置为 75px (750px / 10) 就可以

里面页面元素 rem 值：页面元素的 px 值 / 75

剩余的，让 flexible.js 去算

github 地址：https://github.com/amfe/lib-flexible

