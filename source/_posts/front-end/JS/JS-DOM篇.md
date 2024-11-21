---
title: JS - DOM篇
date: 2022-02-26
cover: assets/JS.png
categories:
- [前端, JavaScript]
tags:
  - JavaScript
  - 前端
---

# Web APIs 简介

## Web APIs 和 JS 基础关联性

**JS 基础阶段：**

+   学习的是 ECMAScript 标准规定的基本语法
+   要求掌握 JS 基础语法
+   只学习基本语法，做不了常用的网页交互效果
+   目的是为了 JS 后面的课程打基础、做铺垫

**Web APIs 阶段：**

+   Web APIs 是 W3C 组织的标准
+   Web APIs 主要学习 DOM 和 BOM
+   Web APIs 是 JS 所独有的部分
+   主要学习页面交互功能
+   需要使用 JS 基础的课程内容做基础

JS 基础学习 ECMAScript 基础语法为后面作铺垫，Web APIs 是 JS 的应用，大量使用 JS 基础语法做交互效果

## API 和 Web API

API

API（Application Programming Interface, 应用程序编程接口）是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问源码，或理解内部工作机制的细节。

简单理解：**API 是给程序员提供的一种工具，以便能更轻松的实现想要完成的功能。**

### Web API

**Web API 是浏览器**提供的一套操作浏览器功能和页面元素的 API (BOM 和 DOM)。

现阶段我们主要针对于浏览器讲解常用的 API , 主要针对浏览器做交互效果。

比如我们想要浏览器弹出一个警示框，直接使用 alert (‘弹出’)

MDN 详细 API : https://developer.mozilla.org/zh-CN/docs/Web/API

因为 Web API 很多，所以我们将这个阶段称为 **Web APIs**

# DOM 简介

## 什么是 DOM

文档对象模型（Document Object Model，简称 DOM），是 W3C 组织推荐的处理可扩展标记语言（HTML 或者 XML）的标准编程接口。

W3C 已经定义了一系列的 DOM 接口，通过这些 DOM 接口可以改变网页的内容、结构和样式。

## DOM 树

+   文档：一个页面就是一个文档，DOM 中使用 document 表示
+   元素：页面中的所有标签都是元素，DOM 中使用 element 表示
+   节点：网页中的所有内容都是节点（标签、属性、文本、注释等），DOM 中使用 node 表示

**DOM 把以上内容都看做是对象**

# 获取元素

DOM 在实际开发中主要用来操作元素。

## 根据 ID 获取

使用 getElementById () 方法可以获取带有 ID 的元素对象。

```js
document.getElementById('id');
```

补充：使用 console.dir () 可以打印我们获取的元素对象，更好的查看对象里面的属性和方法。

## 根据标签名获取

使用 getElementsByTagName () 方法可以返回带有指定标签名的对象的集合。

```js
document.getElementsByTagName('标签名');
```

注意：

1.  因为得到的是一个对象的集合，所以我们想要操作里面的元素就需要遍历。
2.  得到元素对象是动态的

## 通过 HTML5 新增的方法获取

1.  ```js
    document.getElementsByClassName('类名')；// 根据类名返回元素对象集合
    ```
    
2.  ```js
    document.querySelector('选择器');// 根据指定选择器返回第一个元素对象
    ```
    
    ```html
    <div class="box">盒子1</div>
    <div class="box">盒子2</div>
    <div id="nav">
        <ul>
            <li>首页</li>
            <li>产品</li>
        </ul>
    </div>
    ```
    
    ```js
    var firstBox = document.querySelector('.box');
    var nav = document.querySelector('#nav');
    var li = document.querySelector('li');
    ```
    
3.  ```js
    document.querySelectorAll('选择器');// 根据指定选择器返回
    ```

## 获取特殊元素（body，html）

**获取 body 元素**

```js
doucumnet.body// 返回 body 元素对象
```

**获取 html 元素**

```js
document.documentElement// 返回 html 元素对象
```

# 事件基础

JavaScript 使我们有能力创建动态页面，而事件是可以被 JavaScript 侦测到的行为。

简单理解：触发 --- 响应机制。

网页中的每个元素都可以产生某些可以触发 JavaScript 的事件，例如，我们可以在用户点击某按钮时产生一个事件，然后去执行某些操作。

## 事件三要素

1.  事件源（谁）
2.  事件类型（什么事件）
3.  事件处理程序（做啥）

> 📗**案例：点击按钮弹出警示框**
>
> 页面中有一个按钮，当鼠标点击按钮的时候，弹出 “你好吗” 警示框。
>
> 1.  获取事件源（按钮）
> 2.  注册事件（绑定事件），使用 onclick
> 3.  编写事件处理程序，写一个函数弹出 alert 警示框
>
> ```js
> var btn = document.getElementById('btn');
> btn.onclick = function() {
>    alert('你好吗');
> };
> ```

## 执行事件的步骤

1.  获取事件源
2.  注册事件（绑定事件）
3.  添加事件处理程序（采取函数赋值形式）

## 常见的鼠标事件

| 鼠标事件    | 触发条件         |
| ----------- | ---------------- |
| onclick     | 鼠标点击左键触发 |
| onmouseover | 鼠标经过触发     |
| onmouseout  | 鼠标离开触发     |
| onfocus     | 获得鼠标焦点触发 |
| onblur      | 失去鼠标焦点触发 |
| onmousemove | 鼠标移动触发     |
| onmouseup   | 鼠标弹起触发     |
| onmousedown | 鼠标按下触发     |

# 操作元素

JavaScript 的 DOM 操作可以改变网页内容、结构和样式，我们可以利用 DOM 操作元素来改变元素里面的内容、属性等。注意以下都是属性

## 改变元素内容

1.  ```js
    element.innerText
    ```
    
    从起始位置到终止位置的内容，但它不识别 html 标签，去除空格和换行
    
    ```html
    <div></div>
    <p>
        我是文字
        <span>123</span>
    </p>
    ```
    
    ```js
    var div = document.querySelector('div');
    div.innerText = '<strong>今天是：</strong> 2023';
    // 页面会显示 & lt;strong > 和 & lt;/strong>，且没有加粗效果
    var p = document.querySelector('p');
    console.log(p.innerText);
    // 控制台不显示 & lt;span > 和 & lt;/span>，且 “我是文字” 和 “<span>123</span>” 在同一行，它俩前面的空格也没有了
    ```
    
2.  ```js
    element.innerHTML
    ```
    
    起始位置到终止位置的全部内容，包括 html 标签，同时保留空格和换行
    
    ```js
    var div = document.querySelector('div');
    div.innerHTML = '<strong>今天是：</strong> 2023';
    // 页面不显示 & lt;strong > 和 & lt;/strong>，有加粗效果
    var p = document.querySelector('p');
    console.log(p.innerHTML);
    // 控制台显示 & lt;span > 和 & lt;/span>，且 “我是文字” 和 “<span>123</span>” 之间有换行，它俩前面的空格都保留
    ```

## 常用元素的属性操作

1.  innerText、innerHTML 改变元素内容
2.  src、href
3.  id、alt、title

```html
<button id="ldh">刘德华</button>
<button id="zxy">张学友</button><br>
<img src="images/ldh.jpg" alt="" titile="刘德华">
```

```js
var ldh = document.getElementById('ldh');
var zxy = document.getElementById('zxy');
var img = document.querySelector('img');
zxy.onclick = function() {
    img.src = 'images/zxy.jpg';
    img.title = '张学友';
}
ldh.onclick = function() {
    img.src = 'images/ldh.jpg';
    img.title = '刘德华';
}
```

> 📗**案例：分时显示不同图片，显示不同问候语**
>
> 根据不同时间，页面显示不同图片，同时显示不同的问候语。
>
> 如果上午时间打开页面，显示上午好，显示上午的图片。
>
> 如果下午时间打开页面，显示下午好，显示下午的图片。
>
> 如果晚上时间打开页面，显示晚上好，显示晚上的图片。
>
> **案例分析**
>
> 1.  根据系统不同时间来判断，所以需要用到日期内置对象
> 2.  利用多分支语句来设置不同的图片
> 3.  需要一个图片，并且根据时间修改图片，就需要用到操作元素 src 属性
> 4.  需要一个 div 元素，显示不同问候语，修改元素内容即可
>
> ```html
> <img src="images/s.gif" alt="">
> <div>上午好</div>
> ```
>
> ```js
> var img = document.querySelector('img');
> var div = document.querySelector('div');
> // 得到当前的小时数
> var date = new Date();
> var h = date.getHours();
> // 判断小时数，改变图片和文字信息
> if (h < 12) {
>    img.src = 'images/s.gif';
>    div.innerHTML = '亲，上午好，好好写代码';
> }
> else if (h < 18) {
>    img.src = 'images/x.gif';
>    div.innerHTML = '亲，下午好，好好写代码';
> }
> else {
>    img.src = 'images/w.gif';
>    div.innerHTML = '亲，晚上好，好好写代码';
> }
> ```

## 表单元素的属性操作

利用 DOM 可以操作如下表单元素的属性：

type、value、checked、selected、disabled

> 📗**案例：仿京东显示密码**
>
> 点击按钮将密码框切换为文本框，并可以查看密码明文。
>
> **案例分析**
>
> 1.  核心思路： 点击眼睛按钮，把密码框类型改为文本框就可以看见里面的密码
> 2.  一个按钮两个状态，点击一次，切换为文本框，继续点击一次切换为密码框
> 3.  算法：利用一个 flag 变量，来判断 flag 的值，如果是 1 就切换为文本框，flag 设置为 0，如果是 0 就切换为密码框，flag 设置为 1
>
> ```html
> <div class="box">
>    <label for="">
>        <img src="images/close.png" alt="" id="eye">
>    </label>
>    <input type="password" name="" id="pwd">
> </div>
> ```
>
> ```js
> var eye = document.getElementById('eye');
> var pwd = document.getElementById('pwd');
> var flag = 0;
> eye.onclick = function() {
>    if(flag == 0) {
>        pwd.type = 'text';
>        flag = 1;
>        eye.src = 'images/open.png';
>    }
>    else {
>        pwd.type = 'password';
>        flag = 0;
>        pwd.src = 'images/close.png';
>    }
> }
> ```

## 样式属性操作

可以通过 JS 修改元素的大小、颜色、位置等样式。

+   ```js
    element.style// 行内样式操作
    ```
    
    **注意：**
    
    1.  JS 里面的样式采取驼峰命名法比如 fontSize、backgroundColor
    2.  JS 修改 style 样式操作，产生的是行内样式，CSS 权重比较高
    
    ```html
    <div></div>
    ```
    
    ```css
    div {
        width: 200px;
        height: 200px;
        background-color: pink;
    }
    ```
    
    ```js
    var div = document.querySelector('div');
    div.onclick = function() {
        this.style.backgroundColor = 'purple';
        this.style.width = '250px';
    }
    ```
    
    > **📗案例：淘宝点击关闭二维码**
    >
    > 当鼠标点击二维码关闭按钮的时候，则关闭整个二维码。
    >
    > 1.  核心思路： 利用样式的显示和隐藏完成，display:none 隐藏元素 display:block 显示元素
    > 2.  点击按钮，就让这个二维码盒子隐藏起来即可
    >
    > ```js
    > var btn = document.querySelector('.close-btn');
    > var box = document.querySelector('.box');
    > btn.onclick = function() {
    > box.style.display = 'none';
    > }
    > ```
    
    > 🍏**案例：循环精灵图背景**
    >
    > 可以利用 for 循环设置一组元素的精灵图背景
    >
    > 1.  首先精灵图图片排列有规律的
    > 2.  核心思路：利用 for 循环 修改精灵图片的背景位置 background-position
    > 3.  让循环里面的 i 索引号 \* 44 就是每个图片的 y 坐标
    >
    > ```html
    > <div class="box">
    >    <ul>
    >        <li></li>
    >        ...
    >        <li></li>
    >        <!-- 共 12 个 li 标签 -->
    >    </ul>
    > </div>
    > ```
    >
    > ```js
    > var lis = document.querySelectorAll('li');
    > for (var i = 0; i < lis.length; i++) {
    >    var index = i * 44;
    >    lis[i].style.backgroundPosition = '0 -' + index + 'px';
    > 
    > ```
    
    > **💚案例：显示隐藏文本框内容**
    >
    > 当鼠标点击文本框时，里面的默认文字隐藏，当鼠标离开文本框时，里面的文字显示。
    >
    > 1.  首先表单需要 2 个新事件，获得焦点 onfocus 失去焦点 onblur
    > 2.  如果获得焦点，判断表单里面内容是否为默认文字，如果是默认文字，就清空表单内容
    > 3.  如果失去焦点，判断表单内容是否为空，如果为空，则表单内容改为默认文字
    >
    > ```html
    > <input type="text" value="手机">
    > ```
    >
    > ```js
    > var text = document.querySelector('input');
    > text.onfocus = function() {
    >    if(this.value === '手机') {
    >        this.value = '';
    >    }
    >    // 获得焦点需要把文本框里的文字颜色变黑
    >    this.style.color = '#333';
    > }
    > text.onblur = function() {
    >    if(this.value === '') {
    >        this.value = '手机';
    >    }
    >    // 失去焦点需要把文本框里的文字颜色变浅
    >    this.style.color = '#999';
    > }
    > ```
    
+   ```js
    element.className// 类名样式操作
    ```
    
    **注意：**
    
    1.  如果样式修改较多，可以采取操作类名方式更改元素样式。
    2.  class 因为是个保留字，因此使用 className 来操作元素类名属性
    3.  className 会直接更改元素的类名，会覆盖原先的类名。
    
    > 📗**案例：密码框格式提示错误信息**
    >
    > 用户如果离开密码框，里面输入个数不是 6~16，则提示错误信息，否则提示输入正确信息
    >
    > 1.  首先判断的事件是表单失去焦点 onblur
    > 2.  如果输入正确则提示正确的信息颜色为绿色，小图标变化
    > 3.  如果输入不是 6 到 16 位，则提示错误信息颜色为红色，小图标变化
    > 4.  因为里面变化样式较多，我们采取 className 修改样式
    >
    > ```html
    > <div class="register">
    >    <input type="password" class="ipt">
    >    <p class="message">请输入6~16位密码</p>
    > </div>
    > ```
    >
    > ```css
    > div {
    >    width: 600px;
    >    margin: 100px auto;
    > }
    > .message {
    >    display: inline-block;
    >    font-size: 12px;
    >    color: #999;
    >    background: url(images/mess.png) no-repeat left center;
    >    padding-left: 20px;
    > }
    > .wrong {
    >    color: red;
    >    background-image: url(images/wrong.png);
    > }
    > .right {
    >    color: green;
    >    background-image: url(images/right.png);
    > }
    > ```
    >
    > ```js
    > var ipt = document.querySelector('.ipt');
    > var message = document.querySelector('.message');
    > ipt.onblur = function() {
    >    if (this.value.length < 6 || this.value.length > 16) {
    >        message.className = 'message wrong';
    >        message.innerHTML = '您输入的位数不对，要求6~16位';
    >    }
    >    else {
    >        message.className = 'message right';
    >        message.innerHTML = '您输入得正确';
    >    }
    > }
    > ```
    

## 排他思想

如果有同一组元素，我们想要某一个元素实现某种样式，需要用到循环的排他思想算法：

1.  所有元素全部清除样式
2.  给当前元素设置样式
3.  注意顺序不能颠倒

> 📗**案例：百度换肤**
>
> 1.  这个案例练习的是给一组元素注册事件
> 2.  给 4 个小图片利用循环注册点击事件
> 3.  当我们点击了这个图片，让我们页面背景改为当前的图片
> 4.  核心算法：把当前图片的 src 路径取过来，给 body 做为背景即可
>
> ```html
> <ul class="baidu">
>    <li><img src="images/1.jpg"></li>
>    <li><img src="images/2.jpg"></li>
>    <li><img src="images/3.jpg"></li>
>    <li><img src="images/4.jpg"></li>
> </ul>
> ```
>
> ```js
> var imgs = document.querySelector('.baidu').querySelectorAll('img');
> // 循环注册事件
> for (var i = 0; i < imgs.length; i++) {
>    imgs[i].onclick = function {
>        document.body.style.backgroundImage = 'url(' + this.src + ')';
>    }
> }
> ```

> 🍏**案例：表格隔行变色**
>
> 1.  用到新的鼠标事件鼠标经过 onmouseover 鼠标离开 onmouseout
> 2.  核心思路：鼠标经过 tr 行，当前的行变背景颜色，鼠标离开去掉当前的背景颜色
> 3.  注意：第一行（thead 里面的行）不需要变换颜色，因此我们获取的是 tbody 里面的行
>
> ```HTML
> <table>
>    <thead>
>        <tr>
>            <th></th>...<th></th>
>        </tr>
>    </thead>
>    <tbody>
>        <tr>
>            <td></td>...<td></td>
>        </tr>
>        ...
>        <tr>
>            <td></td>...<td></td>
>        </tr>
>    </tbody>
> </table>
> ```
>
> ```css
> .bg {
>    background-color: skyblue;
> }
> ```
>
> ```js
> var trs = document.querySelector('tbody').querySelector('tr');
> for (var i = 0; i < trs.length; i++) {
>    trs[i].onmouseover = function() {
>        this.className = 'bg';
>    }
>    trs[i].onmouseout = function() {
>        this.className = '';
>    }
> }
> ```

> 💚**案例：表单全选取消全选案例**
>
> **业务需求：**
>
> 1.  点击上面全选复选框，下面所有的复选框都选中（全选）
> 2.  再次点击全选复选框，下面所有的复选框都不中选（取消全选）
> 3.  如果下面复选框全部选中，上面全选按钮就自动选中
> 4.  如果下面复选框有一个没有选中，上面全选按钮就不选中
> 5.  所有复选框一开始默认都没选中状态
>
> **案例分析：**
>
> 1.  全选和取消全选做法： 让下面所有复选框的 checked 属性（选中状态）跟随全选按钮即可
> 2.  下面复选框需要全部选中，上面全选才能选中做法：给下面所有复选框绑定点击事件，每次点击，都要循环查看下面所有的复选框是否有没选中的，如果有一个没选中的，上面全选就不选中。
> 3.  可以设置一个变量，来控制全选是否选中
>
> ```html
> <table>
>    <thead>
>        <tr>
>            <th><input type="checkbox" id="j_cbAll" /></th><th>商品</th><th>价钱</th>
>        </tr>
>    </thead>
>    <tbody id="j_tb">
>        <tr>
>            <td><input type="checkbox" /></td><td>iPhone8</td><td>8000</td>
>        </tr>
>        <tr>
>            <td><input type="checkbox" /></td><td>iPad Pro</td><td>5000</td>
>        </tr>
>        <tr>
>            <td><input type="checkbox" /></td><td>iPad Air</td><td>2000</td>
>        </tr>
>        <tr>
>            <td><input type="checkbox" /></td><td>Apple Watch</td><td>2000</td>
>        </tr>
>    </tbody>
> </table>
> ```
>
> ```js
> // 1. 全选和取消全选做法： 让下面所有复选框的 checked 属性（选中状态）跟随全选按钮即可
> var j_cbAll = document.getElementById('j_cbAll');
> var j_tbs = document.getElementById('j_tb').getElementsByTagName('input');
> j_cbAll.onclick = function() {
>    //this.checked 可以得到当前复选框的选中状态，如果是 true 就是选中，如果是 false 就是未选中
>    for (var i = 0; i < j_tbs.length; i++) {
>        j_tbs[i].checked = this.checked;
>    }
> }
> // 2. 下面复选框需要全部选中，上面全选才能选中做法：给下面所有复选框绑定点击事件，每次点击，都要循环查看下面所有的复选框是否有没选中的，如果有一个没选中的，上面全选就不选中。
> for (var i = 0; i < j_tbs.length; i++) {
>    j_tbs[i].onclick = function() {
>        //flag 控制全选按钮是否选中
>        var flag = true;
>        for (var i = 0; i < j_tbs.length; i++) {
>            if (!j_tbs[i].checked) {
>                flag = false;
>                break;// 只要有一个没有选中，剩下的就无需循环判断了
>            }
>        }
>        j_cbAll.checked = flag;
>    }
> }
> ```

## 自定义属性的操作

1.  **获取属性值**
    
    +   `element.属性` 获取属性值。
        
        ```html
        <div id="demo" index="1"></div>
        ```
        
        ```js
        var div = document.querySelector('div');
        console.log(div.id);// demo
        ```
        
        获取内置属性值（元素本身自带的属性），如 id、class 等
        
    +   `element.getAttribute('属性');`
        
        ```js
        console.log(div.getAttribute('index'));// 1
        ```
        
        主要获得自定义的属性（标准）我们程序员自定义的属性，如上面的 index
    
2.  **设置属性值**
    
    +   `element.属性=‘值’` 设置内置属性值。
        
    +   `element.setAttribute('属性','值');`
        
        主要设置自定义的属性（标准）
    
3.  **移除属性**
    
    +   `element.removeAttribute('属性');`

> 📗**案例：tab 栏切换**
>
> 当鼠标点击上面相应的选项卡（tab），下面内容跟随变化
>
> 1.  Tab 栏切换有 2 个大的模块
> 2.  上面的模块选项卡，点击某一个，当前这一个底色会是红色，其余不变（排他思想）修改类名的方式
> 3.  下面的模块内容，会跟随上面的选项卡变化。所以下面模块变化写到点击事件里面。
> 4.  规律：下面的模块显示内容和上面的选项卡一一对应，相匹配。
> 5.  核心思路：给上面的 tab\_list 里面的所有小 li 添加自定义属性，属性值从 0 开始编号。
> 6.  当我们点击 tab\_list 里面的某个小 li，让 tab\_con 里面对应序号的内容显示，其余隐藏（排他思想）
>
> ```html
> <div class="tab">
>    <div class="tab_list">
>        <ul>
>            <li class="current">商品介绍</li>
>            <li>规格与包装</li>
>            <li>售后保障</li>
>            <li>商品评价（50000）</li>
>            <li>手机社区</li>
>        </ul>
>    </div>
>    <div class="tab_con">
>        <div class="item" style="display: block"><!-- display: block 除了改为块级元素之外，还有可以显示的意思 -->
>            商品介绍模块内容
>        </div>
>        <div class="item">
>            规格与包装模块内容
>        </div>
>        <div class="item">
>            售后保障模块内容
>        </div>
>        <div class="item">
>            商品评价（50000）模块内容
>        </div>
>        <div class="item">
>            手机社区
>        </div>
>    </div>
> </div>
> ```
>
> ```css
> .tab_list .current {
>    background-color: red;
>    color: #fff;
> }
> .item {
>    display: none;/* 元素会被完全从文档流中移除，不占据页面布局空间 */
> }
> ```
>
> ```js
> var tab_list = document.querySelector('.tablist');
> var lis = tab_list.querySelectorAll('li');
> var items = document.querySelectorAll('.item');
> for (var i = 0; i < lis.length; i++) {
>    // 开始给 5 个小 li 设置索引号
>    lis[i].setAttribute('index', i); 
>    lis[i].onclick = function() {
>        // 1. 上面的模块选项卡，点击某一个，当前这一个底色会是红色，其余不变（排他思想）修改类名的方式
>        // 其余的 li 清除 current 这个类
>        for (var i = 0; i < lis.length; i++) {
>            lis[i].className = '';
>        }
>        // 被点击的 li 留下类名
>        this.className = 'current';
>        // 2. 下面的显示内容模块
>        var index = this.getAttribute('index');
>        // 让其余的 item 类 div 隐藏
>        for (var i = 0; i < items.length; i++) {
>            items[i].style.display = 'none';
>        }
>        // 留下对应的 item 显示出来
>        items[index].style.display = 'block';
>    }
> }
> ```

## H5 自定义属性

**自定义属性目的：是为了保存并使用数据。有些数据可以保存到页面中而不用保存到数据库中。**

自定义属性获取是通过 getAttribute (‘属性’) 获取。

但是有些自定义属性很容易引起歧义，不容易判断是元素的内置属性还是自定义属性。

H5 给我们新增了自定义属性：

1.  **设置 H5 自定义属性**
    
    H5 规定自定义属性 data - 开头作为属性名并且赋值。
    
    比如 `<div data-index=“1”></div>`
    
    或者使用 JS 设置 `element.setAttribute(‘data-index’, 2)`
    
2.  **获取 H5 自定义属性**
    
    1.  兼容性获取 `element.getAttribute(‘data-index’);`
        
    2.  H5 新增 `element.dataset.index` 或者 `element.dataset[‘index’]`
        
        ```html
        <div data-index="2" data-list-name="andy"></div>
        ```
        
        ```js
        var div = document.querySelector('div');
        //dataset 是一个集合，里面存放了所有 data - 开头的自定义属性
        console.log(div.dataset);
        console.log(div.dataset.index);
        // 如果自定义属性里有多个 - 连接的单词，获取的时候采取驼峰命名法
        console.log(div.dataset.listName);
        console.log(div.dataset['listName']);
        ```

# 节点操作

获取元素通常使用两种方式：

1.  **利用 DOM 提供的方法获取元素**
    +   document.getElementById()
    +   document.getElementsByTagName()
    +   document.querySelector 等
    +   逻辑性不强、繁琐
2.  **利用节点层级关系获取元素**
    +   利用父子兄节点关系获取元素
    +   逻辑性强，但是兼容性稍差

这两种方式都可以获取元素节点，我们后面都会使用，但是节点操作更简单

## 节点概述

网页中的所有内容都是节点（标签、属性、文本、注释等），在 DOM 中，节点使用 node 来表示。

HTML DOM 树中的所有节点均可通过 JavaScript 进行访问，所有 HTML 元素（节点）均可被修改，也可以创建或删除。

一般地，节点至少拥有 nodeType（节点类型）、nodeName（节点名称）和 nodeValue（节点值）这三个基本属性。

+   元素节点 nodeType 为 1
+   属性节点 nodeType 为 2
+   文本节点 nodeType 为 3（文本节点包含文字、空格、换行等）

在实际开发中，节点操作主要操作的是**元素节点**

## 节点层级

利用 DOM 树可以把节点划分为不同的层级关系，常见的是**父子兄弟层级关系**。

### 父级节点

```js
node.parentNode
```

+   parentNode 属性可返回某节点的父节点，注意是最近的一个父节点
+   如果指定的节点没有父节点则返回 null

### 子节点

1.  ```js
    parentNode.childNodes//（标准）
    ```
    
    parentNode.childNodes 返回包含指定节点的子节点的集合，该集合为即时更新的集合。
    
    **注意：** 返回值里面包含了所有的子节点，包括元素节点，文本节点等。
    
    如果只想要获得里面的元素节点，则需要专门处理。所以我们一般不提倡使用 childNodes
    
    ```js
    var ul = document. querySelector(‘ul’);
    for(var i = 0; i < ul.childNodes.length;i++) {
        if (ul.childNodes[i].nodeType == 1) {
            //ul.childNodes [i] 是元素节点
            console.log(ul.childNodes[i]);
        }
    }
    ```
    
2.  ```js
    parentNode.children//（非标准）
    ```
    
    parentNode.children 是一个只读属性，返回所有的子元素节点。它只返回子元素节点，其余节点不返回（**这个是我们重点掌握的**）。
    
    虽然 children 是一个非标准，但是得到了各个浏览器的支持，因此我们可以放心使用
    
    1.  如果想要第一个子元素节点，可以使用 `parentNode.chilren[0]`
    2.  如果想要最后一个子元素节点，可以使用 `parentNode.chilren[parentNode.chilren.length -1]`
3.  ```js
    	parentNode.firstChild
    ```
    
    firstChild 返回第一个子节点，找不到则返回 null。同样，也是包含所有的节点。
    
4.  ```js
    parentNode.lastChild
    ```
    
    lastChild 返回最后一个子节点，找不到则返回 null。同样，也是包含所有的节点。
    
5.  ```js
    parentNode.firstElementChild
    ```
    
    firstElementChild 返回第一个子元素节点，找不到则返回 null。
    
6.  ```js
    parentNode.lastElementChild
    ```
    
    lastElementChild 返回最后一个子元素节点，找不到则返回 null。
    

> 📗**案例：下拉菜单**
>
> 1.  导航栏里面的 li 都要有鼠标经过效果，所以需要循环注册鼠标事件
> 2.  核心原理：当鼠标经过 li，里面的第二个孩子 ul 显示，当鼠标离开，则 ul 隐藏
>
> ```html
> <ul class="nav">
>    <li>
>        <a href="#">微博</a>
>        <ul>
>            <li><a href="#">私信</a></li>
>            <li><a href="#">评论</a></li>
>            <li><a href="#">@我</a></li>
>        </ul>
>    </li>
>    ...
>    <li>
>        <a href="#">微博</a>
>        <ul>
>            <li><a href="#">私信</a></li>
>            <li><a href="#">评论</a></li>
>            <li><a href="#">@我</a></li>
>        </ul>
>    </li>
> </ul>
> ```
>
> ```css
> .nav li {
>    display: none;
> }
> ```
>
> ```js
> var nav = document.querySelector('.nav');
> var lis = nav.children;
> for (var i = 0; i < lis.length; i++) {
>    lis[i].onmouseover = function() {
>        this.children[1].style.display = 'block';
>    }
>    lis[i].onmouseout = function() {
>        this.children[1].style.display = 'none';
>    }
> }
> ```

### 兄弟节点

1.  ```js
    node.nextSibling
    ```
    
    nextSibling 返回当前元素的下一个兄弟节点，找不到则返回 null。同样，也是包含所有的节点。
    
2.  ```js
    node.previousSibling
    ```
    
    previousSibling 返回当前元素上一个兄弟节点，找不到则返回 null。同样，也是包含所有的节点。
    
3.  ```js
    node.nextElementSibling
    ```
    
    nextElementSibling 返回当前元素下一个兄弟元素节点，找不到则返回 null。
    
4.  ```js
    
    ```
    
    previousElementSibling 返回当前元素上一个兄弟节点，找不到则返回 null。
    

## 创建节点

1.  ```js
    document.createElement('tagName')
    ```
    
    document.createElement () 方法创建由 tagName 指定的 HTML 元素。因为这些元素原先不存在，是根据我们的需求动态生成的，所以我们也称为动态创建元素节点。
    
    ```html
    <div class="create"></div>
    ```
    
    ```js
    var create = document.querySelector('.create');
    for (var i = 0; i <= 100; i++) {
        var a = document.createElement('a');
        create.appendChild(a);
    }
    ```
    
    createElement () 创建多个元素效率稍低一点点，但是结构更清晰
    
2.  ```js
    document.write()
    ```
    
    直接将内容写入页面的内容流
    
    ```html
    <button>点击</button>
    <p>abc</p>
    ```
    
    ```js
    document.write('<div>123</div>');
    // "123" 会显示在 “abc” 下方
    ```
    
    但是文档流执行完毕（整个页面解析完成）再去调用 document.write ()，会导致页面全部重绘，只剩下 document.write () 括号内的内容
    
    ```js
    var btn = document.querySelector('button');
    btn.onclick = function() {
        document.write('<div>123</div>');
    }
    // 点击按钮后页面只剩 “123”
    //window.onload 是整个页面加载完了，再去调用里面的 js
    ```
    
    ```js
    window.onload = function() {
        document.write('<div>123</div>');
    }
    // 页面一加载出来就只剩 “123” 了
    ```
    
3.  ```js
    element.innerHTML
    ```
    
    将内容写入某个 DOM 节点，不会导致页面全部重绘
    
    ```html
    <div class="inner"></div>
    ```
    
    ```js
    var inner = document.querySelector('.inner');
    // 拼接字符串
    /* 
    for (var i = 0; i <= 100; i++) {
        inner.innerHTML += '<a href="#"> 百度 & lt;/a>';
    }
    */
    // 数组形式拼接
    var arr = [];
    for (var i = 0; i <= 100; i++) {
        arr.push('<a href="#">百度</a>');
    }
    inner.innerHTML = arr.join('');
    ```
    
    innerHTML 创建多个元素效率更高（不要拼接字符串，采取数组形式拼接），结构稍微复杂
    
    innerHTML 效率要比 creatElement 高
    

## 添加节点

1.  ```js
    node.appendChild(child)
    ```
    
    node.appendChild () 方法将一个节点添加到指定父节点的子节点列表末尾。类似于 CSS 里面的 after 伪元素。
    
2.  ```js
    node.insertBefore(child, 指定元素)
    ```
    
    node.insertBefore () 方法将一个节点添加到父节点的指定子节点前面。类似于 CSS 里面的 before 伪元素。
    

> 📗**案例：简单版发布留言案例**
>
> 1.  核心思路：点击按钮之后，就动态创建一个 li，添加到 ul 里面。
> 2.  创建 li 的同时，把文本域里面的值通过 li.innerHTML 赋值给 li
> 3.  如果想要新的留言后面显示就用 appendChild，如果想要前面显示就用 insertBefore
>
> ```html
> <textarea name="" id="">123</textarea>
> <button>发布</button>
> <ul></ul>
> ```
>
> ```js
> var btn = document.querySelector('button');
> var text = document.querySelector('textarea');
> var ul = document.querySelector('ul');
> btn.onclick = function() {
>    if(text.value == '') {
>        alert('您没有输入内容');
>        return false;// 终止这个操作
>    }
>    else {
>        // 1. 创建元素
>    	var li = document.createElement('li');
>        li.innerHTML = text.value;
>        // 2. 添加元素
>        // ul.appendChild(li);
>        ul.insertBefore(li, ul.children[0]);
>    }
>    
> }
> ```

## 删除节点

```js
node.removeChild(child)
```

node.removeChild () 方法从 DOM 中删除一个子节点，返回删除的节点。

> 📗**案例：删除留言案例**
>
> 1.  当我们把文本域里面的值赋值给 li 的时候，多添加一个删除的链接
>
> 2.  需要把所有的链接获取过来，当我们点击当前的链接的时候，删除当前链接所在的 li
>
> 3.  阻止链接跳转需要添加 javascript:void (0); 或者 javascript:;
>
>
> （若添加的是 #，则当前页面的 url 最后会多出一个 #）
>
> ```css
> li a {
>    float: right;
> }
> ```
>
> ```js
> 	
> btn.onclick = function() {
>    if(text.value == '') {
>        alert('您没有输入内容');
>        return false;// 终止这个操作
>    }
>    else {
>        // 1. 创建元素
>    	var li = document.createElement('li');
>        li.innerHTML = text.value + '<a href="javascript:;">删除</a>';
>        // 2. 添加元素
>        // ul.appendChild(li);
>        ul.insertBefore(li, ul.children[0]);
>        // 3. 删除元素，删除的是当前链接的父亲 li
>        var as = document.querySelector('a');
>        for (var i = 0; i < as.length; i++) {
>            as[i].onclick = function() {
>                ul.removeChild(this.parentNode);
>            }
>        }
>    }
> }
> ```

## 复制节点（克隆节点）

```js
node.cloneNode()
```

node.cloneNode () 方法返回调用该方法的节点的副本。也称为克隆节点 / 拷贝节点

**注意：**

1.  如果括号参数为空或者为 false，则是浅拷贝，即只克隆复制节点本身，不克隆里面的子节点。
2.  如果括号参数为空或者为 false，则是浅拷贝，即只克隆复制节点本身，不克隆里面的子节点。

```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
```

```js
var ul = document.querySelector('ul');
var lili = ul.children[0].cloneNode();
ul.appendChild(lili);
```

> 📗**案例：动态生成表格**
>
> 1.  因为里面的学生数据都是动态的，我们需要 js 动态生成。这里我们模拟数据，自己定义好数据。数据我们采取对象形式存储。
> 2.  所有的数据都是放到 tbody 里面的行里面。
> 3.  因为行很多，我们需要循环创建多个行（对应多少人）
> 4.  每个行里面又有很多单元格（对应里面的数据），我们还继续使用循环创建多个单元格，并且把数据存入里面（双重 for 循环）
> 5.  最后一列单元格是删除，需要单独创建单元格。
> 6.  最后添加删除操作，单击删除，可以删除当前行。
>
> ```html
> <table cellspacing="0">
>    <thead>
>        <tr>
>            <th>姓名</th><th>科目</th><th>成绩</th><th>操作</th>
>        </tr>
>    </thead>
>    <tbody>
>    </tbody>
> </table>
> ```
>
> ```js
> // 1. 先去准备好学生的数据
> var datas = [{
>    name: '李欣怡',
>    subject: 'JavaScript',
>    score: 100
> }, {
>    name: '弘历',
>    subject: 'JavaScript',
>    score: 98
> }, {
>    name: '乾隆',
>    subject: 'JavaScript',
>    score: 99
> }, {
>    name: '李隆基',
>    subject: 'JavaScript',
>    score: 88
> }];
> // 2. 往 tbody 里面创建行，有几个人（通过数组的长度）就创建几行
> var tbody = document.querySelector('tbody');
> for (var i = 0; i < datas.length; i++) {
>    // 创建 tr 行
>    var tr = document.createElement('tr');
>    tbody.appendChild(tr);
>    // 3. 行里面创建单元格 td
>    for (var k in datas[i]) {//k 得到的是属性名，obj [k] 得到属性值
>        var td = document.createElement('td');
>        td.innerHTML = datas[i][k];
>        tr.appendChild(td);
>    }
>    // 4. 创建有 “删除” 2 字的单元格
>    var td = document.createElement('td');
>    td.innerHTML = '<a href="javascript:;">删除</a>';
>    tr.appendChild(td);
> }
> // 5. 删除操作
> var as = document.querySelectorAll('a');
> for (var i = 0; i < as.length; i++) {
>    as[i].onclick = function() {
>        tbody.removeChild(this.parentNode.parentNode);
>    }
> }
> ```

# 事件高级

## 注册事件（绑定事件）

给元素添加事件，称为注册事件或者绑定事件。

注册事件有两种方式：传统方式和方法监听注册方式

### 传统注册方式

+   利用 on 开头的事件 onclick
    
+   `<button onclick=“alert('hi~')”></button>`
    
+   `btn.onclick = function() {}`
    
+   特点：注册事件的**唯一性**
    
+   同一个元素同一个事件只能设置一个处理函数，最后注册的处理函数将会覆盖前面注册的处理函数
    

```html
<button>传统注册事件</button>
```

```js
var btns = document.querySelectorAll('button');
btns[0].oncilck = function() {
    alert('hi');
}
btns[0].oncilck = function() {
    alert('hao a u');
}
// 最后只会弹出 “hao a u”
```

### 方法监听注册方式

+   w3c 标准推荐方式
+   addEventListener () 是一个方法
+   特点：同一个元素同一个事件可以注册多个监听器
+   按注册顺序依次执行

1.  **addEventListener 事件监听方式**
    
    ```js
    eventTarget.addEventListener(type, listener[, useCapture])
    ```
    
    eventTarget.addEventListener () 方法将指定的监听器注册到 eventTarget（目标对象）上，当该对象触发指定的事件时，就会执行事件处理函数。
    
    该方法接收三个参数：
    
    +   type：事件类型字符串，比如 click 、mouseover ，注意这里不要带 on
    +   listener：事件处理函数，事件发生时，会调用该监听函数
    +   useCapture：可选参数，是一个布尔值，默认是 false。
    
    ```html
    <button>方法监听注册事件</button>
    ```
    
    ```js
    var btn = document.querySelector('button');
    btn.addEventListener('click', function() {
        alert(22);
    });
    btn.addEventListener('click', function() {
        alert(33);
    });
    // 先弹出 22，再弹出 33
    ```
    
2.  **attachEvent 事件监听方式**
    
    ```js
    eventTarget.attachEvent(eventNameWithOn, callback)
    ```
    
    eventTarget.attachEvent () 方法将指定的监听器注册到 eventTarget（目标对象）上，当该对象触发指定的事件时，指定的回调函数就会被执行。
    
    该方法接收两个参数：
    
    +   eventNameWithOn：事件类型字符串，比如 onclick 、onmouseover ，这里要带 on
    +   callback：事件处理函数，当目标触发事件时回调函数被调用
    
    ```html
    <button>attachEvent</button>
    ```
    
    ```js
    var btn = document.querySelector('button');
    btn.attachEvent('onclick', function() {
        alert(11);
    })
    ```

## 删除事件（解绑事件）

### 传统注册方式

eventTarget.onclick = null;

```html
<div></div>
```

```js
var div = document.querySelector('div');
div.onclick = function() {
    alert(11);
    // 只有第一次点击的时候弹出，之后点击不再弹出
    div.onclick = null;
}
```

### 方法监听注册方式

1.  eventTarget.removeEventListener(type, listener\[, useCapture\]);
    
    ```html
    <div></div>
    ```
    
    ```js
    var div = document.querySelector('div');
    div.addEventListener('click', fn);
    function fn() {
        alert(22);
        div.removeEventListener('click', fn);
    }
    ```
    
2.  eventTarget.detachEvent(eventNameWithOn, callback);
    
    ```html
    <div></div>
    ```
    
    ```js
    var div = document.querySelector('div');
    div.attachEvent('onclick', fn1);
    function fn1() {
        alert(33);
        div.detachEvent('onclick', fn1);
    }
    ```

## DOM 事件流

事件流描述的是从页面中接收事件的顺序。

事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程即 DOM 事件流。

比如我们给一个 div 注册了点击事件：

DOM 事件流分为 3 个阶段：

1.  捕获阶段
    
    由 DOM 最顶层节点开始，然后逐级向下传播到到最具体的元素接收的过程。
    
2.  当前目标阶段
    
3.  冒泡阶段
    
    事件开始时由最具体的元素接收，然后逐级向上传播到 DOM 最顶层节点的过程。
    

我们向水里面扔一块石头，首先它会有一个下降的过程，这个过程就可以理解为从最顶层向事件发生的最具体元素（目标点）的捕获过程；之后会产生泡泡，会在最低点（最具体元素）之后漂浮到水面上，这个过程相当于事件冒泡。

**注意：**

1.  JS 代码中只能执行捕获或者冒泡其中的一个阶段。
2.  onclick 和 attachEvent 只能得到冒泡阶段。
3.  addEventListener (type, listener \[, useCapture\]) 第三个参数如果是 true，表示在事件捕获阶段调用事件处理程序；如果是 false（不写默认就是 false），表示在事件冒泡阶段调用事件处理程序。
4.  实际开发中我们很少使用事件捕获，我们更关注事件冒泡。
5.  有些事件是没有冒泡的，比如 onblur、onfocus、onmouseenter、onmouseleave
6.  事件冒泡有时候会带来麻烦，有时候又会帮助很巧妙的做某些事件

## 事件对象

### 什么是事件对象

```js
eventTarget.onclick = function(event) {}
eventTarget.addEventListener('click', function(event) {})
// 这个 event 就是事件对象，我们还喜欢的写成 e 或者 evt
//event 是形参，系统帮我们设定为事件对象，不需要传递实参过去。
// 当我们注册事件时，event 对象就会被系统自动创建，并依次传递给事件监听器（事件处理函数）。
```

官方解释：event 对象代表事件的状态，比如键盘按键的状态、鼠标的位置、鼠标按钮的状态。

简单理解：事件发生后，跟事件相关的一系列信息数据的集合都放到这个对象里面，这个对象就是事件对象 event，它有很多属性和方法。

比如：

1.  谁绑定了这个事件。
2.  鼠标触发事件的话，会得到鼠标的相关信息，如鼠标位置。
3.  键盘触发事件的话，会得到键盘的相关信息，如按了哪个键。

### 事件对象的常见属性和方法

e.target 和 this 的区别：

+   this 返回的是绑定事件的对象（元素）
    
+   e.target 返回的是触发事件的对象（元素）
    

```html
<ul>
    <li>abc</li>
    <li>abc</li>
    <li>abc</li>
</ul>
```

```js
var ul = document.querySelector('ul');
ul.addEventListener('click', function(e) {
    // 我们给 ul 绑定了事件，那么 this 就指向 ul
    console.log(this);
    //e.target 指向我们点击的那个对象，谁触发了这个事件，我们点击的是 li，e.target 指向的就是 li
    console.log(e.target);
})
```

| 事件对象属性方法    | 说明                                                   |
| ------------------- | ------------------------------------------------------ |
| e.target            | 返回触发事件的对象，标准                               |
| e.srcElement        | 返回触发事件的对象，非标准                             |
| e.type              | 返回事件的类型，比如 click、mouseover，不带 on         |
| e.cancelBubble      | 该属性阻止冒泡，非标准                                 |
| e.returnValue       | 该属性阻止默认事件（默认行为），非标准                 |
| e.preventDefault()  | 该方法阻止默认事件（默认行为），标准，比如不让链接跳转 |
| e.stopPropagation() | 阻止冒泡，标准                                         |

```html
<a href="http://www.baidu.com">百度</a>
```

```js
var a = document.querySelector('a');
a.addEventListener('click', function(e) {
    e.preventDefault();
})
// 传统的注册方式
a.onclick = function(e) {
    // 方法
    e.preventDefault();
    // 属性
    e.returnValue;
    //return false 也能阻止默认行为，return 后面的代码不执行了，而且只限于传统的注册方式
    return false;
    alert(11);
}
```

## 阻止事件冒泡

事件冒泡：开始时由最具体的元素接收，然后逐级向上传播到到 DOM 最顶层节点。

事件冒泡本身的特性，会带来坏处，也会带来好处，需要我们灵活掌握。

+   标准写法：利用事件对象里面的 stopPropagation () 方法
    
    ```js
    e.stopPropagation()
    ```
    
+   非标准写法：利用事件对象 cancelBubble 属性
    
    ```js
    e.cancelBubble = true;
    ```

## 事件委托（代理、委派）

```html
<ul>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
</ul>
```

点击每个 li 都会弹出对话框，以前需要给每个 li 注册事件，是非常辛苦的，而且访问 DOM 的次数越多，这就会延长整个页面的交互就绪时间。

**事件委托**

事件委托也称为事件代理，在 jQuery 里面称为事件委派。

**事件委托的原理**

不是每个子节点单独设置事件监听器，而是事件监听器设置在其父节点上，然后利用冒泡原理影响设置每个子节点。

以上案例：给 ul 注册点击事件，然后利用事件对象的 target 来找到当前点击的 li，因为点击 li，事件会冒泡到 ul 上，ul 有注册事件，就会触发事件监听器。

```js
var ul = document.querySelector('ul');
ul.addEventListener('click', function(e) {
    e.target.style.backgroundColor = 'pink';
})
```

**事件委托的作用**

我们只操作了一次 DOM，提高了程序的性能。

## 鼠标事件

### 常用的鼠标事件

| 鼠标事件    | 触发条件         |
| ----------- | ---------------- |
| onclick     | 鼠标点击左键触发 |
| onmouseover | 鼠标经过触发     |
| onmouseout  | 鼠标离开触发     |
| onfocus     | 获得鼠标焦点触发 |
| onblur      | 失去鼠标焦点触发 |
| onmousemove | 鼠标移动触发     |
| onmouseup   | 鼠标弹起触发     |
| onmousedown | 鼠标按下触发     |

1.  禁止鼠标右键菜单
    
    contextmenu 主要控制应该何时显示上下文菜单，主要用于程序员取消默认的上下文菜单
    
    ```js
    document.addEventListener('contextmenu', function(e) {
        e.preventDefault();
    })
    ```
    
2.  禁止鼠标选中（selectstart 开始选中）
    
    ```js
    document.addEventListener('selectstart', function(e) {
    	e.preventDefault();
    })
    ```

### 鼠标事件对象

event 对象代表事件的状态，跟事件相关的一系列信息的集合。现阶段我们主要是用鼠标事件对象 MouseEvent 和键盘事件对象 KeyboardEvent。

| 鼠标事件对象 | 说明                                    |
| ------------ | --------------------------------------- |
| e.clientX    | 返回鼠标相对于浏览器窗口可视区的 X 坐标 |
| e.clientY    | 返回鼠标相对于浏览器窗口可视区的 Y 坐标 |
| e.pageX      | 返回鼠标相对于文档页面的 X 坐标         |
| e.pageY      | 返回鼠标相对于文档页面的 Y 坐标         |
| e.screenX    | 返回鼠标相对于电脑屏幕的 X 坐标         |
| e.screenY    | 返回鼠标相对于电脑屏幕的 Y 坐标         |

> **📗案例：跟随鼠标的天使**
>
> 某个天使图片一直跟随鼠标移动
>
> 1.  鼠标不断的移动，使用鼠标移动事件：mousemove
> 2.  在页面中移动，给 document 注册事件
> 3.  图片要移动距离，而且不占位置，我们使用绝对定位即可
> 4.  核心原理：每次鼠标移动，我们都会获得最新的鼠标坐标，把这个 x 和 y 坐标做为图片的 top 和 left 值就可以移动图片
>
> ```html
> <img src="images/angel.gif" alt="">
> ```
>
> ```css
> img {
>    position: absolute;
> }
> ```
>
> ```js
> document.addEventListener('mousemove', function(e) {
>    var x = e.pageX;
>    var y = e.pageY;
>    var pic = document.querySelector('img');
>    // 不要忘记给 left 和 top 添加 px 单位
>    pic.style.left = x - 50 + 'px';
>    pic.style.top = y - 40 + 'px';
> })
> ```

## 键盘事件

### 常用键盘事件

事件除了使用鼠标触发，还可以使用键盘触发，注意给文档 document 添加键盘事件

| 键盘事件   | 触发条件                       |
| ---------- | ------------------------------ |
| onkeyup    | 某个键盘按键被松开时触发       |
| onkeydown  | 某个键盘按键被按下时触发       |
| onkeypress | 某个键盘按键被按下并弹起时触发 |

**注意：**onkeypress 和前面 2 个的区别是，它不识别功能键，比如左右箭头，shift 等。

### 键盘事件对象

| 键盘事件对象 | 说明                |
| ------------ | ------------------- |
| keyCode      | 返回该键的 ASCII 值 |

**注意：**onkeypress 和前面 2 个的区别是，它不识别功能键，比如左右箭头，shift 等。

> 📗**案例：模拟京东按键输入内容**
>
> 当我们按下 s 键，光标就定位到搜索框
>
> 1.  核心思路：检测用户是否按下了 s 键，如果按下 s 键，就把光标定位到搜索框里面
> 2.  使用键盘事件对象里面的 keyCode 判断用户按下的是否是 s 键
> 3.  搜索框获得焦点：使用 js 里面的 focus () 方法
>
> ```html
> <input type="text">
> ```
>
> ```js
> var search = document.querySelector('input');
> document.addEventListener('keyup', function(e) {// 用 'keydown' 的话，会把一个 s 输入搜索框
>    //console.log (e.keyCode);// 测 s 键的 ASCII 码
>    if(e.keyCode == 83) {
>        search.focus();
>    }
> })
> ```

> **🍏案例：模拟京东快递单号查询**
>
> 要求：当我们在文本框中输入内容时，文本框上面自动显示大字号的内容。
>
> 1.  快递单号输入内容时，上面的大号字体盒子（con）显示，这里面的字号更大
> 2.  表单检测用户输入：给表单添加键盘事件
> 3.  同时把快递单号里面的值（value）获取过来赋值给 con 盒子（innerText）作为内容
> 4.  如果快递单号里面内容为空，则隐藏大号字体盒子（con）盒子
> 5.  注意：keydown 和 keypress 在文本框里面的特点：他们两个事件触发的时候，文字还没有落入文本框中
> 6.  keyup 事件触发的时候，文字已经落入文本框里面了
> 7.  当我们失去焦点，就隐藏这个 con 盒子
> 8.  当我们获得焦点，并且文本框内容不为空，就显示这个 con 盒子
>
> ```html
> <div class="search">
>    <div class="con">123</div>
>    <input type="text" placeholder="请输入您的快递单号" class="jd">
> </div>
> ```
>
> ```css
> .con {
>    display: none;
> }
> /* con 盒子下的白色小三角形 */
> .con::before {
>    content: '';
>    width: 0;
>    height: 0;
>    position: absolute;
>    top: 28px;
>    left: 18px;
>    border: 8px solid #000;
>    border-style: solid dashed dashed;
>    border-color: #fff transparent transparent;
> }
> ```
>
> ```js
> var con = document.querySelector('.con');
> var jd_input = document.querySelector('.jd');
> jd_input.addEventListener('keyup', function() {// 用 'keydown' 或 'keypress' 的话，输入第一个字符触发 keydown 或 keypress 的时候，文字还没落入文本框，事件结束了文字才落入文本框，这时文本框内有内容，但触发 keydown 或 keypress 事件时文本框里相当于是空值，con 元素不显示。这两个事件触发时文本框的 value 值总是比实际输入并显示的字符串少一个字符
>    if (this.value == '') {
>        con.style.display = 'none';
>    }
>    else {
>        con.style.display = 'block';
>    	con.innerText = this.value;
>    }
> })
> jd_input.addEventListener('blur', function() {
>    con.style.display = 'none';
> })
> jd_input.addEventListener('focus', function() {
>    if (this.value !== '') {
>        con.style.display = 'block';
>    }
> })
> ```

