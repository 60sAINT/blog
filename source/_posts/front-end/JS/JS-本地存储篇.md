---
title: JS - 本地存储篇
date: 2022-03-03
cover: assets/JS.png
categories:
- [前端, JavaScript]
tags:
  - JavaScript
  - 前端
---

随着互联网的快速发展，基于网页的应用越来越普遍，同时也变的越来越复杂，为了满足各种各样的需求，会经常性在本地存储大量的数据，HTML5 规范提出了相关解决方案。

**本地存储特性**

1.  数据存储在用户浏览器中
2.  设置、读取方便、甚至页面刷新不丢失数据
3.  容量较大， `sessionStorage` 约 5M、 `localStorage` 约 20M
4.  只能存储字符串，可以将对象 `JSON.stringify()` 编码后存储

# `window.sessionStorage`

1.  生命周期为关闭浏览器窗口（就算刷新页面数据也不会丢失）
2.  在同一个窗口 (页面) 下数据可以共享
3.  以键值对的形式存储使用

**存储数据：**

```js
sessionStorage.setItem(key, value)
```

存储的数据可以在此查看：

**获取数据：**

```js
sessionStorage.getItem(key)
```

**删除数据：**

```js
sessionStorage.removeItem(key)
```

**删除所有数据：**

```js
sessionStorage.clear()
```

```html
<input type="text">
<button class="set">存储数据</button>
<button class="get">获取数据</button>
<button class="remove">删除数据</button>
<button class="del">清空所有数据</button>
```

```js
var ipt = document.querySelector('input');
var set = document.querySelector('.set');
var get = document.querySelector('.get');
var remove = document.querySelector('.remove');
var del = document.querySelector('.del');
// 存储数据
set.addEventListener('click', function() {
    var val = ipt.value;
    sessionStorage.setItem('uname', val);
    sessionStorage.setItem('pwd', val);
});
// 获取数据
get.addEventListener('click', function() {
    console.log(sessionStorage.getItem('uname'));
});
// 删除数据
remove.addEventListener('click', function() {
    sessionStorage.removeItem('uname');
});
// 删除所有数据
del.addEventListener('click', function() {
    sessionStorage.clear();
});
```

# `window.localStorage`

1.  声明周期永久生效，除非手动删除，否则关闭页面也会存在
2.  可以多窗口（页面）共享（同一浏览器可以共享）
3.  以键值对的形式存储使用

**存储数据：**

```js
localStorage.setItem(key, value)
```

**获取数据：**

```js
localStorage.getItem(key)
```

**删除数据：**

```js
localStorage.removeItem(key)
```

**删除所有数据：**

```js
localStorage.clear()
```

> 📗**案例：记住用户名**
>
> 如果勾选记住用户名，下次用户打开浏览器，就在文本框里面自动显示上次登录的用户名
>
> 1.  把数据存起来，用到本地存储
> 2.  关闭页面，也可以显示用户名，所以用到 `localStorage`
> 3.  打开页面，先判断是否有这个用户名，如果有，就在表单里面显示用户名，并且勾选复选框
> 4.  当复选框发生改变的时候 change 事件
> 5.  如果勾选，就存储，否则就移除
>
> ```html
> <input type="text" id="username"><input type="checkbox" name="" id="remember"> 记住用户名
> ```
>
> ```js
> var username = document.querySelector('#username');
> var remember = document.querySelector('#remember');
> if (localStorage.getItem('username')) {
>    username.value = localStorage.getItem('username');
>    remember.checked = true;
> }
> remember.addEventListener('change', function() {
>    if(this.checked) {
>        localStorage.setItem('username', username.value);
>    }
>    else {
>        localStorage.removeItem('username');
>    }
> })
> ```

