---
title: AJAX - 入门篇
date: 2023-08-02
cover: assets/AJAX.png
categories:
- [前端, AJAX]
tags:
  - JavaScript
  - 前端
  - AJAX
---

# AJAX - 入门篇

# AJAX 概念和 axios 使用

## 什么是 AJAX

定义：AJAX \[ˈeɪdʒæks\] 是异步的 JavaScript 和 XML（**A**synchronous **J**avaScript **A**nd **X**ML）。简单点说，就是使用 `XMLHttpRequest` 对象与服务器通信。它可以使用 JSON，XML，HTML 和 text 文本等格式发送和接收数据。AJAX 最吸引人的就是它的 “异步” 特性，也就是说它可以在不重新刷新页面的情况下与服务器通信，交换数据，或更新页面。

概念：AJAX 是浏览器与服务器进行数据通信的技术

## 怎么用 AJAX

1.  先使用 axios \[æk‘sioʊs\] 库，与服务器进行数据通信
    +   基于 XMLHttpRequest 封装、代码简单、月下载量在 14 亿次
    +   Vue、React 项目中都会用到 axios
2.  再学习 XMLHttpRequest 对象的使用，了解 AJAX 底层原理

## axios 使用

语法：

1.  引入 axios.js：https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js
    
2.  使用 axios 函数
    
    +   传入配置对象
    +   再用 .then 回调函数接收结果，并做后续处理
    
    ```js
    axios({
    	url: '目标资源地址'
    }).then((result) => {
    	// 对服务器返回的数据做后续处理
    })
    ```

> 📗
>
> 需求：请求目标资源地址，拿到省份列表数据，显示到页面
>
> 目标资源地址：http://hmajax.itheima.net/api/province
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802095404.png)
>
> ```html
> <head>
>    <!-- 引入 axios 库 -->
>    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
> </head>
> <body>
>    <p class="my-p"></p>
> </body>
> ```
>
> ```JS
> // 使用 axios 函数
> axios({
>    url: 'http://hmajax.itheima.net/api/province'
> }).then(result => {
>    // 好习惯：多打印，确认属性名
>    console.log(result);
>    console.log(result.data.list);
>    console.log(result.data.list.join('<br>'));
>    // 把准备好的省份列表插入到页面
>    document.querySelector('.my-p').innerHTML = result.data.list.join('<br>');
> })
> ```
>
> 控制台打印如下：
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/eeffa77a7f52fc64c0ef6993010a26b.jpg)

# 认识 URL

知道作用和组成，方便与后端人员沟通

## 什么是 URL

定义：

**统一资源定位符**（英语：**U**niform **R**esource **L**ocator，缩写：**URL**，或称**统一资源定位器、定位地址、URL 地址**）俗称网页地址，简称**网址**，是因特网上标准的资源的地址（Address），如同在网络上的门牌。它最初是由蒂姆・伯纳斯 - 李发明用来作为万维网的地址，现在它已被万维网联盟编制为因特网标准 RFC 1738。

例如：

+   https://www.baidu.com/index.html
    
    网页资源
    
+   https://www.itheima.com/images/logo.png
    
    图片资源
    
+   http://hmajax.itheima.net/api/province
    
    数据资源
    

概念：

URL 就是统一资源定位符，简称网址，用于访问网络上的资源

## URL 的组成

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802101615.png)

### 协议

http 协议：超文本传输协议，规定浏览器和服务器之间传输数据的格式

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802101755.png)

协议范围：http，https，...

### 域名

标记服务器在互联网中方位

例如：

+   百度服务器 www.baidu.com
+   淘宝服务器 www.taobao.com

### 资源路径

标记资源在服务器下的具体位置

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802102210.png)

> 📗获取 - 新闻列表
>
> 需求：使用 axios 从服务器拿到新闻列表数据
>
> 目标资源地址：http://hmajax.itheima.net/api/news
>
> ```html
> <head>
>    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
> </head>
> ```
>
> ```js
> axios({
>    url: 'http://hmajax.itheima.net/api/news'
> }).then(result => {
>    cosnole.log(result);
> })
> ```
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/de00985cb4b9a1953dbcaf09f70fd08.jpg)

## URL 查询参数

定义：浏览器提供给服务器的额外信息，让服务器返回浏览器想要的数据

语法：http://xxxx.com/xxx/xxx?参数名 1 = 值 1&参数名 2 = 值 2

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802103146.png)

> 📗axios－查询参数
>
> 语法：使用 axios 提供的 params 选项
>
> 注意：axios 在运行时把参数名和值，会拼接到 url? 参数名 = 值
>
> 城市列表：http://hmajax.itheima.net/api/city?pname = 河北省
>
> ```html
> <head>
>    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
> </head>
> <body>
>    <p></p>
> </body>
> ```
>
> ```js
> axios({
>    url: 'http://hmajax.itheima.net/api/city',
>    // 查询参数
>    params: {
>        pname: '河北省'
>    }
> }).then(result => {
> 	console.log(result.data.list);
>    document.querySelector('p').innerHTML = result.data.list.join('<br>');
> })
> ```

> 🍏案例 - 地区查询
>
> 需求：根据输入的省份名字和城市名字，查询地区并渲染列表
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/985b3a58a7060ad20dbd572998ad36c.jpg)
>
> 首先：确定 URL 网址和参数说明
>
> +   查询某个省内某个城市的所有地区: http://hmajax.itheima.net/api/area
> +   参数名：
> +   pname：省份名字或直辖市名字，比如北京、福建省、辽宁省...
> +   cname：城市名字，比如北京市、厦门市、大连市...
>
> 完整：http://hmajax.itheima.net/api/area?pname = 北京 & cname = 北京市
>
> ```html
> <head>
>    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
> </head>
> ```
>
> ```js
> // 1. 查询按钮 - 点击事件
> document.querySelector('.sel-btn').addEventListener('click', () => {
>    // 2. 获取省份和城市名字
>    let pname = document.querySelector('.province').value;
>    let cname = document.querySelector('.city').value;
>    // 3. 基于 axios 请求地区列表数据
>    axios({
>        url: 'http://hmajax.itheima.net/api/area',
>        params: {
>            // pname: pname,
>            // cname: cname,
>            // ES6 对象当中属性名和属性值变量同名时可以只写一个，属性名是后端要求的，不能乱写，属性值变量名可以随便定义
>            pname,
>            cname
>        }
>    }).then(result => {
>        // 把数据转成 li 标签插入到页面上
>        let list = result.data.list;
>        let theLi = list.map(areaName => `<li class="list-group-item">${areaName}</li>`).join('');//.join ('') 前的代码返回一个数组，每个元素是 '<li class="list-group-item">${areaName}</li>' 字符串
>        document.querySelector('.list-group').innerHTML = theLi;
>    })
> })
> ```

# 常用请求方法和数据提交

## 常用请求方法

请求方法：对服务器资源，要执行的操作

| 请求方法 | 操作             |
| -------- | ---------------- |
| GET      | 获取数据         |
| POST     | 数据提交         |
| PUT      | 修改数据（全部） |
| DELETE   | 删除数据         |
| PATCH    | 修改数据（部分） |

## 数据提交

场景：当数据需要在服务器上保存

url：请求的 URL 网址

method：请求的方法，GET 为默认值，可以省略（不区分大小写）

data：提交数据

```js
axios({
    url: '目标资源地址',
    method: '请求方法',
    data: {
    	参数名: 值
    }
}).then((result) => {
	// 对服务器返回的数据做后续处理
})
```

> 📗数据提交－注册账号
>
> 需求：通过 axios 提交用户名和密码，完成注册功能
>
> 注册用户 URL 地址：http://hmajax.itheima.net/api/register
>
> 请求方法：POST
>
> 参数名：
>
> username 用户名（中英文和数字组成，最少 8 位）
>
> password 密码（最少 6 位）
>
> ```html
> <head>
>    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
> </head>
> <body>
>    <button class="btn">注册用户</button>
> </body>
> ```
>
> ```js
> document.querySelector('.btn').addEventListener('click', () => {
>    axios({
>        url: 'http://hmajax.itheima.net/api/register',
>        // 指定请求方法
>        method: 'post',
>        // 提交数据
>        data: {
>            username: 'lxy0819',
>            password: '123456'
>        }
>    }).then(result => {
>        console.log(result);
>    })
> })
> ```

## axios 错误处理

场景：再次注册相同的账号，会遇到报错信息

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802130519.png)

处理：用更直观的方式，给普通用户展示错误信息

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802130658.png)

语法：在 then 方法的后面，通过点语法调用 catch 方法，传入回调函数并定义形参

```js
axios({
	// 请求选项
}).then(result => {
	// 处理数据
}).catch(error => {
	// 处理错误
})
```

> 📗注册案例，重复注册时通过弹框提示用户错误原因
>
> ```js
> document.querySelector('.btn').addEventListener('click', () => {
>    axios({
>        url: 'http://hmajax.itheima.net/api/register',
>        // 指定请求方法
>        method: 'post',
>        // 提交数据
>        data: {
>            username: 'lxy0819',
>            password: '123456'
>        }
>    }).then(result => {
>        console.log(result);
>    }).catch(error => {
>        // 处理错误信息
>        console.log(error);
>        console.log(error.response.data.message);
>        alert(error.response.data.message);
>    })
> })
> ```

# HTTP 协议 - 报文

## HTTP 协议 - 请求报文

HTTP 协议：规定了浏览器发送及服务器返回内容的格式

请求报文：浏览器按照 HTTP 协议要求的格式，发送给服务器的内容

```js
axios({
	url: 'http://hmajax.itheima.net/api/register',
	method: 'post',
	data: {
		username: 'itheima007',
		password: '7654321'
	}
})
```

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802141030.png)

请求报文的组成部分有：

1.  请求行：请求方法，URL，协议
    
    ```http
    POST http://hmajax.itheima.net/api/register HTTP/1.1
    ```
    
    请求方法：由代码中的 method 选项设置
    
    URL：由代码中的 url 选项设置
    
    协议：自动携带
    
2.  请求头：以键值对的格式携带的附加信息
    
    请求头里的内容并没有在代码中书写，它是自动生成的
    
    注意这行，规定这次请求时携带的内容类型是一个 json 的字符串：
    
    ```http
    Content-Type: application/json
    ```
    
    所以服务器就会接着往下找，找到空行之后服务器就会知道上面的内容已经告一段落，再往下找就是浏览器发来的内容数据了
    
3.  空行：分隔请求头，空行之后的是发送给服务器的资源
    
4.  请求体：发送的资源
    
    这里是 json 字符串，是在代码当中的 data 项携带的
    

通过 Chrome 的网络面板查看请求报文：

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/88c1653da4bd0a936a7d87f131fd662.jpg)

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802143324.png)

## HTTP - 响应报文

HTTP 协议：规定了浏览器发送及服务器返回内容的格式

响应报文：服务器按照 HTTP 协议要求的格式，返回给浏览器的内容

以下是再次注册已注册的账号后，服务器返回的响应报文：

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802143940.png)

1.  响应行（状态行）：协议、HTTP 响应状态码、状态信息
    
    HTTP 响应状态码：用来表明请求是否成功完成
    
    比如：404（服务器找不到资源）
    
    ```js
    // 响应状态码 404 的可能原因之一是 url 写错了
    axios({
    	url: 'http://hmajax.itheima.net/api/registerweer1ddd',
    	method: 'post',
    	data: {
    		username: 'itheima007',
    		password: '7654321'
    	}
    })
    ```
    
    | 状态码 | 说明       |
    | ------ | ---------- |
    | 1xx    | 信息       |
    | 2xx    | 成功       |
    | 3xx    | 重定向消息 |
    | 4xx    | 客户端错误 |
    | 5xx    | 服务端错误 |
    
2.  响应头：以键值对的格式携带的附加信息，比如：Content-Type
    
3.  空行：分隔响应头，空行之后的是服务器返回的资源
    
4.  响应体：返回的资源
    

通过 Chrome 的网络面板查看响应报文：

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802145010.png)

# 接口文档

**接口文档**：描述接口的文章 （后端工程师）

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802145656.png)

位置 - query：表示参数 pname 和值要在查询参数里传递，写在 params 里面

**接口**：使用 AJAX 和服务器通讯时，使用的 URL，请求方法，以及参数

```js
axios({
    url: 'http://hmajax.itheima.net/api/city',
    method: 'get',
    params: {
        pname: '辽宁省'
    }
})
```

**传送门**：[AJAX 阶段接口文档](https://apifox.com/apidoc/shared-1b0dd84f-faa8-435d-b355-5a8a329e34a8)

> 📗案例 - 注册账号
>
> **接口文档：**
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802150607.png)
>
> Body 参数的意思是在请求体 data 里边把用户名和密码携带  
> application/json 的意思是这次交的请求体数据得是 json 字符串
>
> **接口：**
>
> ```js
> axios({
>    url: 'http://hmajax.itheima.net/api/register',
>    method: 'post',
>    // 在 axios 内部的源码如果发现 data 属性的值是一个对象，内部就会帮你把这个对象转成 json 字符串携带给服务器
>    data: {
>        username: 'lxy0819',
>        password: '123456'
>    }
> })
> ```

> 🍏案例 - 登录
>
> ```html
> <head>
>    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
> </head>
> <body>
>    <button class="btn">用户登录</button>
> </body>
> ```
>
> ```js
> document.querySelector('.btn').addEventListener('click', () => {
>    // 用户登录
>    axios({
>        url: 'http://hmajax.itheima.net/api/login',
>        method: 'post',
>        data: {
>            username: 'lxy0819',
>        	password: '123456'
>        }
>    })
> })
> ```

# 案例 - 用户登录

1.  点击登录时，判断用户名和密码长度
2.  提交数据和服务器通信
3.  提示信息

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802152744.png)

```js
// 目标 1：点击登录时，用户名和密码长度判断，并提交数据和服务器通信
// 目标 2：使用提示框，反馈提示消息
// 2.1 获取提示框
const myAlert = document.querySelector('.alert');
/* 2.2 封装提示框函数，重复调用，满足提示需求
   功能：
   1. 显示提示框
   2. 不同提示文字 msg，和成功绿色失败红色 isSuccess（true 成功，false 失败）
   3. 过 2 秒后，让提示框自动消失
*/
function alertFn(msg, isSuccess) {
    // 显示提示框
    myAlert.classList.add('.show');
    // 提示框内文字
    myAlert.innerText = msg;
    // 提示框背景颜色添加
    const bgStyle = isSuccess ? 'alert-success' : 'alert-danger';
    myAlert.classList.add(bgStyle);
    // 过 2 秒隐藏
    setTimeout(() => {
        myAlert.classList.remove('show');
        // 同时去除提示框背景颜色，避免类名冲突
        myAlert.classList.remove(bgStyle);
    }, 2000);
}
// 1.1 登录 - 点击事件
document.querySelector('.btn-login').addEventListener('click', () => {
    // 1.2 获取用户名和密码
    const username = document.querySelector('.username').value;
    const password = document.querySelector('.password').value;
    // 1.3 判断长度
    if (username.length < 8) {
        alertFn('用户名必须大于等于8位', false);
        console.log('用户名必须大于等于8位');
        return;
    }
    if (password.length < 6) {
        alertFn('密码必须大于等于6位', false);
        console.log('密码必须大于等于6位');
        return;
    }
    // 1.4 基于 axios 提交用户名和密码
    axios({
        url: 'http://hmajax.itheima.net/api/login',
        method: 'post',
        data: {
            username,
            password
        }
    }).then(result => {// 成功才会进入 then
        alertFn(result.data.message, true);
        console.log(result);
        console.log(result.data.message);
    }).catch(error => {// 失败才会进入 catch
        alertFn(error.response.data.message, false);
        console.log(error);
        console.log(error.response.data.message);
    })
})
```

# form-serialize 插件

作用：快速收集表单元素的值

```html
<head>
    <script src="./lib/form-serialize.js"></script>
</head>
<body>
    <form action="javascript:;" class="example-form">
        <input type="text" name="uname">
        <br>
        <input type="text" name="pwd">
        <br>
        <input type="button" class="btn" value="提交">
	</form>
</body>
<!-- 目标：点击提交时，使用 form-serialize 插件，快速收集表单元素值 -->
```

```js
document.querySelector('.btn').addEventListener('click', () => {
    const form = document.querySelector('.example-form');
    /* 参数 1：要获取哪个表单的数据
       参数 2：配置对象
         hash 设置获取数据 data 的结构
           - true：js 对象 {uname: 'lxy0819', pwd: '123456'}（推荐）
           - false：查询字符串 'uname=lxy0819&pwd=123456'
         empty 设置是否获取空值
           - true：获取空值 {uname: '', pwd: '123456'}（推荐，获取到的数据 data 结构与标签结构一致）
           - false：不获取空值 {pwd: '123456'}
    */
    const data = serialize(form, {hash: true, empty: true});
    console.log(data);// {uname: 'lxy0819', pwd: '123456'}。获取到数据对象的属性名来自于 input 元素的 name 属性，建议 name 属性的值和接口文档参数名一致
})
```

> 📗案例 - 用户登录
>
> 使用 form-serialize 插件，收集用户名和密码
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230802154317.png)
>
> ```html
> <!-- 记得在 head 引入插件和 axios 库 -->
> <!-- 表单 -->
> <form class="login-form">
>    <div class="mb-3">
>        <label for="username" class="form-label">账号名</label>
>        <input type="text" class="form-control username" name="username">
>    </div>
>    <div class="mb-3">
>        <label for="password" class="form-label">密码</label>
>        <input type="password" class="form-control password" name="password">
>    </div>
>    <button type="button" class="btn btn-primary btn-login">登录</button>
> </form>
> ```
>
> ```js
> const myAlert = document.querySelector('.alert');
> function alertFn(msg, isSuccess) {
>    myAlert.classList.add('.show');
>    myAlert.innerText = msg;
>    const bgStyle = isSuccess ? 'alert-success' : 'alert-danger';
>    myAlert.classList.add(bgStyle);
>    setTimeout(() => {
>        myAlert.classList.remove('show');
>        myAlert.classList.remove(bgStyle);
>    }, 2000);
> }
> document.querySelector('.btn-login').addEventListener('click', () => {
>    // 使用 serialize 函数，收集登录表单里用户名和密码
>    const form = document.querySelector('.login-form');
>    const data = serialize(form, {hash: true, empty: true});//data 为 {username: 'lxy0819', password: '123456'}
>    // 解构赋值，变量名和变量值相同，可简写
>    // const {username: username, password: password} = data;
>    const {username, password} = data;
>    /* 插件把原本一个一个用 querySelector 获取的代码替换掉了
>    const username = document.querySelector ('.username').value;
>    const password = document.querySelector ('.password').value; 
>    */
>    if (username.length < 8) {
>        alertFn('用户名必须大于等于8位', false);
>        console.log('用户名必须大于等于8位');
>        return;
>    }
>    if (password.length < 6) {
>        alertFn('密码必须大于等于6位', false);
>        console.log('密码必须大于等于6位');
>        return;
>    }
>    axios({
>        url: 'http://hmajax.itheima.net/api/login',
>        method: 'post',
>        data: {
>            username,
>            password
>        }
>    }).then(result => {// 成功才会进入 then
>        alertFn(result.data.message, true);
>        console.log(result);
>        console.log(result.data.message);
>    }).catch(error => {// 失败才会进入 catch
>        alertFn(error.response.data.message, false);
>        console.log(error);
>        console.log(error.response.data.message);
>    })
> })
> ```
