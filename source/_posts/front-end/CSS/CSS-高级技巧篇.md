---
title: CSS - 高级技巧篇
date: 2022-01-27
cover: assets/CSS.png
categories:
- [前端, CSS]
tags:
  - CSS
  - 前端
---

# 精灵图

## 为什么需要精灵图

一个网页中往往会应用很多小的背景图像作为修饰，当网页中的图像过多时，服务器就会频繁地接收和发送请求图片，造成服务器请求压力过大，这将大大降低页面的加载速度。

因此，**为了有效地减少服务器接收和发送请求的次数，提高页面的加载速度**，出现了 CSS 精灵技术（也称 CSS Sprites、CSS 雪碧）。

核心原理：将网页中的一些小背景图像整合到一张大图中，这样服务器只需要一次请求就可以了。

## 精灵图的使用

1.  精灵技术主要针对于背景图片使用。就是把多个小背景图片整合到一张大图片中。
2.  这个大图片也称为 sprites 精灵图 或者雪碧图
3.  移动背景图片位置，此时可以使用 background-position 。
4.  移动的距离就是这个目标图片的 x 和 y 坐标。注意网页中的坐标有所不同
5.  因为一般情况下都是往上往左移动，所以数值是负值。
6.  使用精灵图的时候需要精确测量每个小背景图片的大小和位置。

# 字体图标

**阿里 iconfont 字库** http://www.iconfont.cn

1. 把下载包里面的 fonts 文件夹放入页面根目录下

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230709143349.png)

2. 打开 fonts 文件夹中的 demo.css，复制下列代码到项目的 css 文件

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230709144600.png)

3. html 标签内添加小图标

   `<span class="iconfont">&#xe71f;</span>`


# CSS 三角

网页中常见一些三角形，使用 CSS 直接画出来就可以，不必做成图片或者字体图标。

```css
.triangle {
    width: 0;
    height: 0;
    border: 100px solid transparent;
    border-left-color: pink;
}
/* 以上为直角等边三角形 */
```

**CSS 三角强化**

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230710170641.png)

原理：

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230710170707.png)

把右边的三角形（换为白色）通过定位定到大盒子的右边即可

三角形代码：

```css
width: 0;
height: 0;
border-color: transparent white transparent transparent;
border-width: 22px 8px 0 0;
```

# CSS 用户界面样式

所谓的界面样式，就是更改一些用户操作样式，以便提高更好的用户体验。

## 鼠标样式 cursor

```css
li {cursor: pointer; }
```

设置或检索在对象上移动的鼠标指针采用何种系统预定义的光标形状。

| 属性值      | 描述      |
| ----------- | --------- |
| default     | 小白 默认 |
| pointer     | 小手      |
| move        | 移动      |
| text        | 文本      |
| not-allowed | 禁止      |

## 轮廓线 outline

给表单添加 outline: 0; 或者 outline: none; 样式之后，就可以去掉表单获得焦点时默认的蓝色边框。

```css
input {outline: none; }
```

## 防止拖拽文本域 resize

实际开发中，我们文本域右下角是不可以拖拽的。

```css
textarea { 
    resize: none;
}
```

# vertical-align

CSS 的 vertical-align 属性使用场景：经常用于设置图片或者表单 (行内块元素）和文字垂直对齐。

官方解释：用于设置一个元素的垂直对齐方式，但是它只针对于行内元素或者行内块元素有效。

语法：

```css
vertical-align : baseline | top | middle | bottom
```

| 值       | 描述                                   |
| -------- | -------------------------------------- |
| baseline | 默认。元素放置在父元素的基线上         |
| top      | 把元素的顶端与行中最高元素的顶端对齐   |
| middle   | 把此元素放置在父元素的中部             |
| bottom   | 把元素的底端与行中最低的元素的底端对齐 |

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230710160033.png)

## 图片、表单和文字对齐

图片、表单都属于行内块元素，默认的 vertical-align 是基线对齐。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230710160351.png)

此时可以给图片、表单这些行内块元素的 vertical-align 属性设置为 middle 就可以让文字和图片垂直居中对齐了。

## 解决图片底部默认空白缝隙

bug：图片底侧会有一个空白缝隙，原因是行内块元素会和文字的基线对齐。

主要解决方法有两种：

1.  给图片添加 vertical-align: middle | top| bottom 等。（提倡使用）
2.  把图片转换为块级元素 display: block;

# 溢出的文字省略号显示

1.  **单行文本溢出显示省略号 -- 必须满足三个条件**
    
    ```css
    /*1. 先强制一行内显示文本 */
     white-space: nowrap;/*（ 默认 normal 自动换行） */
     /*2. 超出的部分隐藏 */
     overflow: hidden;
     /*3. 文字用省略号替代超出的部分 */
     text-overflow: ellipsis;
    ```
    
2.  **多行文本溢出显示省略号**
    
    多行文本溢出显示省略号，有较大兼容性问题， 适合于 webKit 内核的浏览器或移动端（移动端大部分是 webkit 内核）
    
    ```css
    overflow: hidden;
    text-overflow: ellipsis;
    /* 弹性伸缩盒子模型显示 */
    display: -webkit-box;
    /* 限制在一个块元素显示的文本的行数 */
    -webkit-line-clamp: 2;
    /* 设置或检索伸缩盒对象的子元素的排列方式 */
    -webkit-box-orient: vertical;
    ```
    
    更推荐让后端人员来做这个效果，因为后端人员可以设置显示多少个字，操作更简单
    

# 常见布局技巧

## margin 负值运用

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230710163104.png)

1.  让每个盒子 margin 往左侧移动 -1px 正好压住相邻盒子边框
    
    ```css
    ul li {
        /* position: relative; */
        float: left;
        list-style: none;
        width: 150px;
        height: 200px;
        border: 1px solid red;
        margin-left: -1px;
    }
    ```
    
2.  鼠标经过某个盒子的时候，提高当前盒子的层级即可（如果没有有定位，则加相对定位（保留位置），如果有定位，则加 z-index）
    
    ```css
    1.如果盒子没有定位，则鼠标经过添加相对定位即可
    ul li:hover {
        border: 1px solid blue;
        position: relative;/* 相对定位会压住其他的标准流或浮动的盒子 */
    }
    ```
    
    ```css
    2.如果li都有定位，则利用z-index提高层级
    ul li:hover {
        border: 1px solid blue;
        z-index: 1;
    }
    ```

## 文字围绕浮动元素

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230710164709.png)

巧妙运用浮动元素不会压住文字的特性

```html
<div class="box">
    <div class="pic">
        <img src="images/img.png" alt="">
    </div>
    <p>
        【集锦】热身赛——巴西0-1秘鲁 内马尔替补两人血染赛场
    </p>
</div>
```

```css
.pic {
    float: left;
    width: 120px;
    height: 60px;
    margin-left: 5px;
}
.pic img {
    width: 100%;
}
```

## 行内块巧妙运用

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230710165708.png)

页码在页面中间显示:

1.  把这些链接盒子转换为行内块， 之后给父级指定 text-align: center;
2.  利用行内块元素中间有缝隙，并且给父级添加 text-align: center; 行内块元素会水平会居中

```html
<div class="box">
    <a href="#">&lt;&lt;上一页</a>
    <a href="#">1</a>
    <a href="#">2</a>
    <a href="#">3</a>
    <a href="#">4</a>
    <a href="#">5</a>
    <a href="#">&gt;&gt;下一页</a>
</div>
```

```css
.box {
    text-align: center;
}
.box a {
    display: inline-block;
    width: 36px;
    height: 36px;
    border: 1px solid #ccc;
}
```

# CSS 初始化

不同浏览器对有些标签的默认值是不同的，为了消除不同浏览器对 HTML 文本呈现的差异，照顾浏览器的兼容，我们需要对 CSS 初始化

简单理解： CSS 初始化是指重设浏览器的样式。 (也称为 CSS reset）

每个网页都必须首先进行 CSS 初始化。

```html
<link rel="stylesheet" href="css/reset.css">
```

**Unicode 编码字体：**

把中文字体的名称用相应的 Unicode 编码来代替，这样就可以有效的避免浏览器解释 CSS 代码时候出现乱码的问题。

比如：

黑体 \\9ED1\\4F53

宋体 \\5B8B\\4F53

微软雅黑 \\5FAE\\8F6F\\96C5\\9ED1

```css
button,
input {
    /* "\5B8B\4F53" 就是宋体的意思，这样浏览器兼容性比较好 */
    font-family: Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif
}
```

# 滤镜 filter

filterCSS 属性将模糊或颜色偏移等图形效果应用于元素。

```css
filter: 函数();
/* 例如： */
filter: blur(5px);	/* blur 模糊处理，数值越大越模糊 */
```

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230712134310.png)

# calc 函数

calc ()：此 CSS 函数让你在声明 CSS 属性值时执行一些计算。

```css
width: calc(100% - 80px);
```

括号里面可以使用 + -\*/ 来进行计算。

# 过渡

过渡（transition) 是 CSS3 中具有颠覆性的特征之一，我们可以在不使用 Flash 动画或 JavaScript 的情况下，当元素从一种样式变换为另一种样式时为元素添加效果。

过渡动画：是从一个状态渐渐的过渡到另外一个状态

可以让我们页面更好看，更动感十足，不会影响页面布局。

经常和:hover 一起搭配使用。

```css
transition: 要过渡的属性 花费时间 运动曲线 何时开始;
```

1. **属性**：想要变化的 CSS 属性，宽度高度背景颜色内外边距都可以。如果想要所有的属性都变化过渡，写一个 all 就可以。
2. **花费时间**：单位是秒（必须写单位）比如 0.5s
3. **运动曲线**：默认是 ease（可以省略）

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230712134810.png)
4. **何时开始**：单位是秒（必须写单位）可以设置延迟触发时间 默认是 0s（可以省略）

# 2D 转换

**转换（transform）** 是 CSS3 中具有颠覆性的特征之一，可以实现元素的位移、旋转、缩放等效果

转换（transform）你可以简单理解为变形

## 二维坐标系

x 轴：水平向右

y 轴：垂直向下

## 移动 translate

2D 移动是 2D 转换里面的一种功能，可以改变元素在页面中的位置，类似**定位**。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230712143120.png)

1.  **语法**
    
    ```css
    transform: translate(x, y);
    /* 或者分开写 */
    transform: translateX(n);
    transform: translateY(n);
    ```
    
2.  **重点**
    
    +   定义 2D 转换中的移动，沿着 X 和 Y 轴移动元素
    +   translate 最大的优点：不会影响到其他元素的位置
    +   translate 中的百分比单位是相对于自身元素的，如 translate:(50%,50%);
    +   对行内标签没有效果

## 旋转 rotate

2D 旋转指的是让元素在 2 维平面内顺时针旋转或者逆时针旋转。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230712143147.png)

1.  **语法**
    
    ```css
    transform: rotate(度数);
    ```
    
2.  **重点**
    
    +   rotate 里面跟度数，单位是 deg 比如 rotate (45deg)
    +   角度为正时，顺时针，负时，为逆时针
    +   默认旋转的中心点是元素的中心点

## 转换中心点 transform-origin

我们可以设置元素转换的中心点

1.  **语法**
    
    ```css
    transform-origin: x y;
    ```
    
2.  **重点**
    
    +   后面的参数 x 和 y 用空格隔开
    +   x y 默认转换的中心点是元素的中心点 (50% 50%)
    +   还可以给 x y 设置像素或者 方位名词（top bottom left right center）

## 缩放 scale

缩放，顾名思义，可以放大和缩小。只要给元素添加上了这个属性就能控制它放大还是缩小。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230712143349.png)

1.  **语法**
    
    ```css
    transform: scale(x, y);
    ```
    
2.  **注意**
    
    +   注意其中的 x 和 y 用逗号分隔
    +   transform: scale (1,1) ：宽和高都放大一倍，相对于没有放大
    +   transform: scale (2,2) ：宽和高都放大了 2 倍
    +   transform: scale (2,2) ：宽和高都放大了 2 倍
    +   transform: scale (0.5,0.5)：缩小
    +   sacle 缩放最大的优势：可以设置转换中心点缩放，默认以中心点缩放的，而且不影响其他盒子

## 2D 转换综合写法

1.  同时使用多个转换，其格式为：transform: translate () rotate () scale () ... 等，
2.  其顺序会影转换的效果。（先旋转会改变坐标轴方向）
3.  **当我们同时有位移和其他属性的时候，记得要将位移放到最前**

# 动画

**动画（animation）** 是 CSS3 中具有颠覆性的特征之一，可通过设置多个节点来精确控制一个或一组动画，常用来实现复杂的动画效果。

相比较过渡，动画可以实现更多变化，更多控制，连续自动播放等效果。

## 动画的基本使用

制作动画分为两步：先定义动画，再使用（调用）动画

1.  **用 keyframes 定义动画（类似定义类选择器）**
    
    ```css
    @keyframes 动画名称 {
        0% {
        	width:100px;
        }
        100% {
        	width:200px;
        }
    }
    ```
    
    **动画序列**
    
    +   0% 是动画的开始，100% 是动画的完成。这样的规则就是动画序列。
    +   在 @keyframes 中规定某项 CSS 样式，就能创建由当前样式逐渐改为新样式的动画效果。
    +   动画是使元素从一种样式逐渐变化为另一种样式的效果。您可以改变任意多的样式任意多的次数。
    +   请用百分比来规定变化发生的时间，或用关键词 "from" 和 "to"，等同于 0% 和 100%。
2.  **元素使用动画**
    
    ```css
    div {
        width: 200px;
        height: 200px;
        background-color: aqua;
        margin: 100px auto;
        /* 调用动画 */
        animation-name: 动画名称;
        /* 持续时间 */
        animation-duration: 持续时间;
    }
    ```

## 动画常用属性

| 属性                      | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| @keyframes                | 规定动画                                                     |
| animation                 | 所有动画属性的简写属性，除了 animation-play-state 属性       |
| animation-name            | 规定 @keyframes 动画的名称                                   |
| animation-duration        | 规定动画完成一个周期所花费的秒或毫秒，默认是 0。（必须的）   |
| animation-timing-function | 规定动画的速度曲线，默认是 “ease”。                          |
| animation-delay           | 规定动画何时开始，默认是 0。                                 |
| animation-iteration-count | 规定动画被播放的次数，默认是 1，还有 infinite                |
| animation-direction       | 规定动画是否在下一周期逆向播放，默认是 “normal“,alternate 逆播放 |
| animation-play-state      | 规定动画是否正在运行或暂停。默认是 "running", 还有 "paused"。 |
| animation-fill-mode       | 规定动画结束后状态，保持 forwards，回到起始 backwards        |

## 动画简写属性

animation：动画名称 持续时间 运动曲线 何时开始 播放次数 是否反方向 动画起始或者结束的状态；

```css
animation: myfirst 5s linear 2s infinite alternate;
```

+   简写属性里面不包含 animation-play-state
+   暂停动画：animation-play-state: paused; 经常和鼠标经过等其他配合使用
+   想要动画走回来，而不是直接跳回来：animation-direction: alternate
+   盒子动画结束后，停在结束位置：animation-fill-mode: forwards

## 速度曲线细节

animation-timing-function：规定动画的速度曲线，默认是 “ease”

| 值          | 描述                                           |
| ----------- | ---------------------------------------------- |
| linear      | 动画从头到尾的速度是相同的。匀速               |
| ease        | 默认。动画以低速开始，然后加快，在结束前变慢。 |
| ease-in     | 动画以低速开始。                               |
| ease-out    | 动画以低速结束。                               |
| ease-in-out | 动画以低速开始和结束。                         |
| steps()     | 指定了时间函数中的间隔数量（步长）             |

# 3D 转换

**特点**

+   近大远小。
+   物体后面遮挡不可见

当我们在网页上构建 3D 效果的时候参考这些特点就能产出 3D 效果。

## 三维坐标系

三维坐标系其实就是指立体空间，立体空间是由 3 个轴共同组成的。

+   x 轴：水平向右注意：x 右边是正值，左边是负值
+   y 轴：垂直向下注意：y 下面是正值，上面是负值
+   z 轴：垂直屏幕注意：往外面是正值，往里面是负值

## 移动 translate3d

3D 移动在 2D 移动的基础上多加了一个可以移动的方向，就是 z 轴方向。

+   transform: translateX (100px)：仅仅是在 x 轴上移动
+   transform: translateY (100px)：仅仅是在 Y 轴上移动
+   transform: translateZ (100px)：仅仅是在 Z 轴上移动（注意：translateZ 一般用 px 单位）
+   transform: translate3d (x,y,z)：其中 x、y、z 分别指要移动的轴的方向的距离

因为 z 轴是垂直屏幕，由里指向外面，所以默认是看不到元素在 z 轴的方向上移动

**透视 perspective**

在 2D 平面产生近大远小视觉立体，但是只是效果二维的

+   如果想要在网页产生 3D 效果需要透视（理解成 3D 物体投影在 2D 平面内）。
+   模拟人类的视觉位置，可认为安排一只眼睛去看
+   透视我们也称为视距：视距就是人的眼睛到屏幕的距离
+   距离视觉点越近的在电脑平面成像越大，越远成像越小
+   透视的单位是像素

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230712153823.png)

**透视写在被观察元素的父盒子上面的**

**d**：就是视距，视距就是一个距离人的眼睛到屏幕的距离。

**z**：就是 z 轴，物体距离屏幕的距离，z 轴越大（正值）我们看到的物体就越大。

**translateZ**

translform: translateZ (100px)：仅仅是在 Z 轴上移动。有了透视，就能看到 translateZ 引起的变化了

+   translateZ：近大远小
+   translateZ：往外是正值
+   translateZ：往里是负值

## 旋转 rotate3d

3D 旋转指可以让元素在三维平面内沿着 x 轴，y 轴，z 轴或者自定义轴进行旋转。

**语法**

+   transform:rotateX (45deg)：沿着 x 轴正方向旋转 45 度
    
+   transform:rotateY (45deg) ：沿着 y 轴正方向旋转 45deg
    
+   transform:rotateZ (45deg) ：沿着 Z 轴正方向旋转 45deg
    
+   transform:rotate3d (x,y,z,deg)：沿着自定义轴旋转 deg 为角度
    
    xyz 是表示旋转轴的矢量，是标示你是否希望沿着该轴旋转，最后一个标示旋转的角度。
    
    +   transform: rotate3d (1,0,0,45deg) 就是沿着 x 轴旋转 45deg
    +   transform: rotate3d (1,1,0,45deg) 就是沿着对角线旋转 45deg

对于元素旋转的方向的判断我们需要先学习一个左手准则。

**左手准则**

+   左手的手拇指指向 x 轴的正方向
+   其余手指的弯曲方向就是该元素沿着 x 轴旋转的方向

## 3D 呈现 transform-style

+   控制子元素是否开启三维立体环境。
+   transform-style: flat 子元素不开启 3d 立体空间 默认的
+   transform-style: preserve-3d; 子元素开启立体空间
+   代码写给父级，但是影响的是子盒子

# 浏览器私有前缀

浏览器私有前缀是为了兼容老版本的写法，比较新版本的浏览器无须添加。

1.  **私有前缀**
    
    +   \-moz-：代表 firefox 浏览器私有属性
    +   \-ms-：代表 ie 浏览器私有属性
    +   \-webkit-：代表 safari、chrome 私有属性
    +   \-o-：代表 Opera 私有属性
2.  **提倡的写法**
    
    ```css
    -moz-border-radius: 10px;
    -webkit-border-radius: 10px;
    -o-border-radius: 10px;
    border-radius: 10px;
    /* 先写私有的 CSS3 属性，再写标准的 CSS3 属性 */
    ```
    
3.  **什么时候去掉私有前缀**
    
    当一个属性成为标准，并且被 Firefox、Chrome 等主流浏览器的最新版普遍兼容的时候，可以去掉该属性的浏览器前缀
    

