---
title: CSS - 页面布局篇
date: 2022-01-25
cover: assets/CSS.png
categories:
- [前端, CSS]
tags:
  - CSS
  - 前端
---

# 盒子模型

## 网页布局的本质

网页布局过程：

1.  先准备好相关的网页元素，网页元素基本都是盒子 Box 。
2.  利用 CSS 设置好盒子样式，然后摆放到相应位置。
3.  往盒子里面装内容.

网页布局的核心本质：就是利用 CSS 摆盒子。

## 盒子模型组成

所谓盒子模型：就是把 HTML 页面中的布局元素看作是一个矩形的盒子，也就是一个盛装内容的容器。

CSS 盒子模型本质上是一个盒子，封装周围的 HTML 元素，它包括：边框、外边距、内边距、和实际内容

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230707164643.png)

## 边框（border）

border 可以设置元素的边框。边框有三部分组成：边框宽度 (粗细)、边框样式、边框颜色

语法：

```css
border : border-width || border-style || border-color
```

| 属性         | 作用                      |
| ------------ | ------------------------- |
| border-width | 定义边框粗细，单位是 `px` |
| border-style | 边框的样式                |
| border-color | 边框颜色                  |

边框样式 border-style 可以设置如下值：

+   none：没有边框，即忽略所有边框的宽度（默认值）
+   solid：边框为单实线 (最为常用的)
+   dashed：边框为虚线
+   dotted：边框为点线

CSS 边框属性允许你指定一个元素边框的样式和颜色。

边框简写：

```css
border: 1px solid red;
/* 没有顺序 */
```

边框分开写法：

```css
border-top: 1px solid red;/* 只设定上边框，其余同理 */
```

**表格的细线边框**

border-collapse 属性控制浏览器绘制表格边框的方式。它控制相邻单元格的边框。

语法：

```css
border-collapse: collapse;
```

+   collapse 单词是合并的意思
+   border-collapse: collapse; 表示相邻边框合并在一起

**边框会影响盒子实际大小**

## 内边距（padding）

padding 属性用于设置内边距，即边框与内容之间的距离。

| 属性           | 作用     |
| -------------- | -------- |
| padding-left   | 左内边距 |
| padding-right  | 右内边距 |
| padding-top    | 上内边距 |
| padding-bottom | 下内边距 |

padding 属性（简写属性）可以有一到四个值。

| 值的个数                     | 表达意思                                                     |
| ---------------------------- | ------------------------------------------------------------ |
| padding: 5px;                | 1 个值，代表上下左右都有 5 像素内边距                        |
| padding: 5px 10px;           | 2 个值，代表上下内边距是 5 像素，左右内边距是 10 像素        |
| padding: 5px 10px 20px;      | 3 个值，代表上内边距是 5 像素，左右内边距是 10 像素，下内边距 20 像素 |
| padding: 5px 10px 20px 30px; | 4 个值，上是 5 像素，右 10 像素，下 20 像素，左是 30 像素，顺时针 |

当我们给盒子指定 padding 值之后，发生了 2 件事情：

1.  内容和边框有了距离，添加了内边距。
2.  padding 影响了盒子实际大小。

如果盒子已经有了宽度和高度，此时再指定内边框，会撑大盒子。

如果盒子本身没有指定 width/height 属性，则此时 padding 不会撑开盒子大小.

## box-sizing

CSS3 中可以通过 box-sizing 来指定盒模型，有 2 个值：即可指定为 content-box、border-box，这样我们计算盒子大小的方式就发生了改变。

可以分成两种情况：

1.  box-sizing: content-box 盒子大小为 width + padding + border（以前默认的）
2.  box-sizing: border-box 盒子大小为 width

如果盒子模型我们改为了 box-sizing: border-box，那 padding 和 border 就不会撑大盒子了（前提 padding 和 border 不会超过 width 宽度）

## 外边距（margin）

margin 属性用于设置外边距，即控制盒子和盒子之间的距离。

| 属性          | 作用     |
| ------------- | -------- |
| margin-left   | 左外边距 |
| margin-right  | 右外边距 |
| margin-top    | 上外边距 |
| margin-bottom | 下外边距 |

margin 简写方式代表的意义跟 padding 完全一致。

### 外边距典型应用

外边距可以让块级盒子**水平居中**，但是必须满足两个条件：

1.  盒子必须指定了宽度（width）。
2.  盒子左右的外边距都设置为 auto 。

```css
.header { 
    width:960px; 
    margin:0 auto;
}
```

常见的写法，以下三种都可以：

+   margin-left: auto;margin-right: auto;
+   margin: auto;
+   margin: 0 auto;

**注意：** 以上方法是让块级元素水平居中，行内元素或者行内块元素水平居中给其父元素添加 text-align: center 即可。

### 外边距合并

使用 margin 定义块元素的垂直外边距时，可能会出现外边距的合并。

主要有两种情况:

1.  **相邻块元素垂直外边距的合并**
    
    当上下相邻的两个块元素（兄弟关系）相遇时，如果上面的元素有下外边距 margin-bottom，下面的元素有上外边距 margin-top，则他们之间的垂直间距不是 margin-bottom 与 margin-top 之和。取两个值中的较大者这种现象被称为相邻块元素垂直外边距的合并。
    
    ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230707171248.png)
    
2.  **嵌套块元素垂直外边距的塌陷**
    
    对于两个嵌套关系（父子关系）的块元素，父元素有上外边距同时子元素也有上外边距，此时父元素会塌陷较大的外边距值。
    
    在只给父元素加上上外边距时能正常：
    
    ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20200707113222816.png)
    
    当给子元素添加一个比父元素小的上外边距时：(margin-top: 10px)
    
    ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20200707113352339.png)
    
    当给子元素添加一个比父元素大的上外边距时：(margin-top: 40px)
    
    ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202007071139163.png)
    
    **解决方案：**
    
    1.  为父元素定义上边框。
    2.  为父元素定义上内边距。
    3.  为父元素添加 overflow: hidden。

## 清除内外边距

网页元素很多都带有默认的内外边距，而且不同浏览器默认的也不一致。因此我们在布局前，首先要清除下网页元素的内外边距。

```css
* {
	padding: 0;/* 清除内边距 */
	margin: 0;/* 清除外边距 */
}
```

**注意：** 行内元素为了照顾兼容性，尽量只设置左右内外边距，不要设置上下内外边距。但是转换为块级和行内块元素就可以了

# 圆角边框

在 CSS3 中，新增了圆角边框样式，这样我们的盒子就可以变圆角了。

border-radius 属性用于设置元素的外边框圆角。

语法：

```css
border-radius: length;
```

+   参数值可以为数值或百分比的形式
+   如果是正方形，想要设置为一个圆，把数值修改为高度或者宽度的一半即可，或者直接写为 50%
+   该属性是一个简写属性，可以跟四个值，分别代表左上角、右上角、右下角、左下角
+   分开写：border-top-left-radius、border-top-right-radius、border-bottom-right-radius 和 border-bottom-left-radius

# 盒子阴影

CSS3 中新增了盒子阴影，我们可以使用 box-shadow 属性为盒子添加阴影。

语法：

```css
box-shadow: h-shadow v-shadow blur spread color inset;
```

| 值       | 描述                                   |
| -------- | -------------------------------------- |
| h-shadow | 必须。水平阴影的位置。允许负值         |
| v-shadow | 必须。垂直阴影的位置。允许负值         |
| blur     | 可选。模糊距离，值越大越模糊           |
| spread   | 可选。阴影的尺寸，值越大阴影面积越大   |
| color    | 可选。阴影的颜色                       |
| inset    | 可选。将外部阴影（outset）改为内部阴影 |

**注意：**

1.  默认的是外阴影 (outset), 但是不可以写这个单词，否则造成阴影无效
2.  盒子阴影不占用空间，不会影响其他盒子排列。

# 文字阴影

在 CSS3 中，我们可以使用 text-shadow 属性将阴影应用于文本。

语法：

```css
text-shadow: h-shadow v-shadow blur color;
```

| 值       | 描述                           |
| -------- | ------------------------------ |
| h-shadow | 必需。水平阴影的位置。允许负值 |
| v-shadow | 必需。垂直阴影的位置。允许负值 |
| blur     | 可选。模糊的距离               |
| color    | 可选。阴影的颜色               |

# 浮动

## 标准流（普通流 / 文档流）

所谓的标准流：就是标签按照规定好默认方式排列.

1.  块级元素会独占一行，从上向下顺序排列。
    +   常用元素：div、hr、p、h1~h6、ul、ol、dl、form、table
2.  行内元素会按照顺序，从左到右顺序排列，碰到父元素边缘则自动换行。
    +   常用元素：span、a、i、em 等

以上都是标准流布局，标准流是最基本的布局方式。

这三种布局方式都是用来摆放盒子的，盒子摆放到合适位置，布局自然就完成了。

## 为什么需要浮动

有很多的布局效果，标准流没有办法完成，此时就可以利用浮动完成布局。因为浮动可以改变元素标签默认的排列方式.

浮动最典型的应用：可以让多个块级元素一行内排列显示。

网页布局第一准则：多个块级元素纵向排列找标准流，多个块级元素横向排列找浮动。

**浮动元素经常和标准流父级搭配使用**

为了约束浮动元素位置，我们网页布局一般采取的策略是:

先用标准流的父元素排列上下位置，之后内部子元素采取浮动排列左右位置。符合网页布局第一准则.

## 什么是浮动

float 属性用于创建浮动框，将其移动到一边，直到左边缘或右边缘触及包含块或另一个浮动框的边缘。

语法：

```css
选择器 {
	float: 属性值;
}
```

| 属性值 | 描述                     |
| ------ | ------------------------ |
| none   | 元素不浮动（**默认值**） |
| left   | 元素向**左**浮动         |
| right  | 元素向**右**浮动         |

## 浮动特性

1.  浮动元素会脱离标准流 (脱标)
    
2.  浮动的元素会一行内显示并且元素顶部对齐
    
    **注意：** 浮动的元素是互相贴靠在一起的（不会有缝隙），如果父级宽度装不下这些浮动的盒子，多出的盒子会另起一行对齐。
    
3.  浮动的元素会具有行内块元素的特性.
    
    任何元素都可以浮动。不管原先是什么模式的元素，添加浮动之后具有**行内块元素**相似的特性。
    
    +   如果块级盒子没有设置宽度，默认宽度和父级一样宽，但是添加浮动后，它的大小根据内容来决定
    +   行内元素同理
4.  浮动的盒子不再保留原先的位置
    

# 常见网页布局

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230707195127.png)

浮动布局注意点：

1.  **浮动和标准流的父盒子搭配。**
    
    先用标准流的父元素排列上下位置，之后内部子元素采取浮动排列左右位置
    
2.  **一个元素浮动了，理论上其余的兄弟元素也要浮动。**
    
    一个盒子里面有多个子盒子，如果其中一个盒子浮动了，那么其他兄弟也应该浮动，以防止引起问题。
    
    浮动的盒子只会影响浮动盒子后面的标准流，不会影响前面的标准流.
    

# 清除浮动

## 为什么需要清除浮动

由于父级盒子很多情况下，不方便给高度，但是子盒子浮动又不占有位置，最后父级盒子高度为 0 时，就会影响下面的标准流盒子。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230707195859.png)

由于浮动元素不再占用原文档流的位置，所以它会对后面的元素排版产生影响

## 清除浮动本质

+   清除浮动的本质是清除浮动元素造成的影响
+   如果父盒子本身有高度，则不需要清除浮动
+   清除浮动之后，父级就会根据浮动的子盒子自动检测高度。父级有了高度，就不会影响下面的标准流了

## 清除浮动方法

语法：

```css
选择器 {
	clear: 属性值;
}
```

| 属性值 | 描述                                       |
| ------ | ------------------------------------------ |
| left   | 不允许左侧有浮动元素（清除左侧浮动的影响） |
| right  | 不允许右侧有浮动元素（清除右侧浮动的影响） |
| both   | 同时清除左右两侧浮动的影响                 |

实际工作中，几乎只用 clear: both;

清除浮动的策略是：闭合浮动

### 额外标签法

**额外标签法**也称为隔墙法，是 W3C 推荐的做法。

额外标签法会在浮动元素末尾添加一个空的标签。例如 `<div style="clear:both"></div>` ，或者其他标签（如 `<br/>` 等）。

```html
<div class="father">
    <div class="floatson1">大毛</div>
    <div class="floatson2">二毛</div>
    <div class="clear"></div>
</div>
<div class="footer"></div>
```

```css
.clear {
    clear: both;
}
```

+   优点：通俗易懂，书写方便
+   缺点：添加许多无意义的标签，结构化较差

注意：要求这个新的空标签必须是块级元素。

### 父级添加 overflow

可以给父级添加 overflow 属性，将其属性值设置为 hidden、auto 或 scroll。

注意是给父元素添加代码

+   优点：代码简洁
+   缺点：无法显示溢出的部分

### :after 伪元素法

:after 方式是额外标签法的升级版。也是给父元素添加

```css
.clearfix:after {
    content: "";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}
```

```html
<div class="father clearfix">
    <div class="floatson1">大毛</div>
    <div class="floatson2">二毛</div>
</div>
<div class="footer"></div>
```

+   优点：没有增加标签，结构更简单
+   代表网站：百度、淘宝网、网易等

### 双伪元素清除浮动

也是给父元素添加

```css
.clearfix:before, .clearfix:after {
	content: "";
	display: table;
}
.clearfix:after {
	clear: both;
}
```

+   优点：代码更简洁
+   代表网站：小米、腾讯等

# 定位

## 为什么需要定位

1.  某个元素可以自由的在一个盒子内移动位置，并且压住其他盒子.
2.  当我们滚动窗口的时候，盒子是固定屏幕某个位置的。

以上效果，标准流或浮动都无法快速实现，此时需要定位来实现

## 定位组成

**定位**：将盒子定在某一个位置，所以**定位也是在摆放盒子，按照定位的方式移动盒子。**

定位 = 定位模式 + 边偏移。

定位模式用于指定一个元素在文档中的定位方式。边偏移则决定了该元素的最终位置。

### 定位模式

定位模式决定元素的定位方式，它通过 CSS 的 position 属性来设置，其值可以分为四个：

| 值         | 语义         |
| ---------- | ------------ |
| `static`   | **静态**定位 |
| `relative` | **相对**定位 |
| `absolute` | **绝对**定位 |
| `fixed`    | **固定**定位 |

### 边偏移

边偏移就是定位的盒子移动到最终位置。有 top、bottom、left 和 right4 个属性。

| 边偏移属性 | 示例           | 描述                                                   |
| ---------- | -------------- | ------------------------------------------------------ |
| `top`      | `top: 80px`    | **顶端**偏移量，定义元素相对于其父元素**上边线的距离** |
| `bottom`   | `bottom: 80px` | **底部**偏移量，定义元素相对于其父元素**下边线的距离** |
| `left`     | `left: 50%`    | **左侧**偏移量，定义元素相对于其父元素**左边线的距离** |
| `right`    | `right: 80px`  | **右侧**偏移量，定义元素相对于其父元素**右边线的距离** |

## 静态定位 static

静态定位是元素的默认定位方式，无定位的意思。

语法：

```css
选择器 {
    position: static;
}
```

+   静态定位按照标准流特性摆放位置，它没有边偏移
+   静态定位在布局时很少用到

## 相对定位 relative

**相对定位**是元素在移动位置的时候，是相对于它原来的位置来说的。

语法：

```css
选择器 {
    position: relative;
}
```

相对定位的特点：

1.  它是相对于自己原来的位置来移动的（移动位置的时候参照点是自己原来的位置）。
2.  原来在标准流的位置继续占有，后面的盒子仍然以标准流的方式对待它。

因此，相对定位并没有脱标。它最典型的应用是给绝对定位当爹的

## 绝对定位 absolute

绝对定位是元素在移动位置的时候，是相对于它祖先元素来说的

语法：

```css
选择器 {
	position: absolute; 
}
```

绝对定位的特点：

1.  如果没有祖先元素或者祖先元素没有定位，则以浏览器为准定位（Document 文档）。
2.  如果祖先元素有定位（相对、绝对、固定定位），则以最近一级的有定位祖先元素为参考点移动位置。
3.  绝对定位不再占有原先的位置。（脱标）

**子绝父相的由来**

定位中最常用的一种方式。这句话的意思是：子级是绝对定位的话，父级要用相对定位。

1.  子级绝对定位，不会占有位置，可以放到父盒子里面的任何一个地方，不会影响其他的兄弟盒子。
2.  父盒子需要加定位限制子盒子在父盒子内显示。
3.  父盒子布局时，需要占有位置，因此父亲只能是相对定位。

相对定位经常用来作为绝对定位的父级。

因为父级需要占有位置，因此是相对定位，子盒子不需要占有位置，则是绝对定位

当然，子绝父相不是永远不变的，如果父元素不需要占有位置，子绝父绝也会遇到。

## 固定定位 fixed

**固定定位**是元素固定于浏览器可视区的位置。主要使用场景：可以在浏览器页面滚动时元素的位置不会改变。

语法：

```css
选择器 {
	position: fixed; 
}
```

固定定位的特点：

1.  以浏览器的可视窗口为参照点移动元素。
    
    +   跟父元素没有任何关系
    +   不随滚动条滚动。
2.  固定定位不再占有原先的位置。
    
    固定定位也是脱标的，其实固定定位也可以看做是一种特殊的绝对定位。
    
3.  固定的盒子必须有宽度，其宽度也与父元素没有关系，以屏幕为准
    

**绝对定位和固定定位会完全压住盒子**

浮动元素不同，只会压住它下面标准流的盒子，但是不会压住下面标准流盒子里面的文字（图片）

但是绝对定位（固定定位）会压住下面标准流所有的内容。

浮动之所以不会压住文字，因为浮动产生的目的最初是为了做文字环绕效果的。文字会围绕浮动元素

## 粘性定位 sticky

粘性定位可以被认为是相对定位和固定定位的混合。Sticky 粘性的

语法：

```css
选择器 {
	position: sticky; 
    top: 10px; 
}
```

粘性定位的特点：

1.  以浏览器的可视窗口为参照点移动元素（固定定位特点）
2.  粘性定位占有原先的位置（相对定位特点）
3.  必须添加 top 、left、right、bottom 其中一个才有效

跟页面滚动搭配使用。

## 定位叠放次序 z-index

在使用定位布局时，可能会出现盒子重叠的情况。此时，可以使用 z-index 来控制盒子的前后次序 (z 轴)

语法：

```css
选择器 {
	z-index: 1; 
}
```

+   数值可以是正整数、负整数或 0, 默认是 auto，数值越大，盒子越靠上
+   如果属性值相同，则按照 HTML 书写顺序，后来居上
+   数字后面不能加单位
+   只有定位的盒子才有 z-index 属性

# 元素的显示与隐藏

## display 属性

display 属性用于设置一个元素应如何显示。

+   display: none；隐藏对象
+   display: block；除了转换为块级元素之外，同时还有显示元素的意思

display 隐藏元素后，不再占有原来的位置。

## visibility 可见性

visibility 属性用于指定一个元素应可见还是隐藏。

+   visibility: visible ; 元素可视
+   visibility: hidden; 元素隐藏

visibility 隐藏元素后，继续占有原来的位置。

## overflow 溢出

overflow 属性指定了如果内容溢出一个元素的框（超过其指定高度及宽度）时，会发生什么。

| 属性值  | 描述                                       |
| ------- | ------------------------------------------ |
| visible | 不剪切内容也不添加滚动条                   |
| hidden  | 不显示超过对象尺寸的内容，超出的部分隐藏掉 |
| scroll  | 不管超出内容否，总是显示横竖两个滚动条     |
| auto    | 超出自动显示滚动条，不超出不显示滚动条     |

一般情况下，我们都不想让溢出的内容显示出来，因为溢出的部分会影响布局。
