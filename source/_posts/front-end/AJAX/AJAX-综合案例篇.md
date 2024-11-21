---
title: AJAX - 综合案例篇
date: 2023-08-03
cover: assets/AJAX.png
categories:
- [前端, AJAX]
tags:
  - JavaScript
  - 前端
  - AJAX
---

# 案例 - 图书管理

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230803103902.png)

1.  Bootstrap 弹框
2.  渲染列表（查）
3.  新增图书（增）
4.  删除图书（删）
5.  编辑图书（改）

## Bootstrap 弹框

Bootstrap 弹框 = Modal = 模态

功能：不离开当前页面，显示单独内容，供用户操作

### 通过属性控制

单纯显示 / 隐藏

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230803104312.png)

步骤：

1.  引入 bootstrap.css
    
2.  准备弹框标签，确认结构
    
3.  通过 bootstrap 内部的自定义属性，控制弹框的显示和隐藏
    
    ```html
    <button data-bs-toggle="modal" data-bs-target="css选择器">显示弹框</button>
    <button data-bs-dismiss="modal">Close</button>
    ```

```html
<head>
    <!-- 引入 bootstrap.css -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- 引入 bootstrap.js -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.min.js"></script>
</head>
<body>
    <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target=".my-box">显示弹框</button>
    <!-- 以下是从 bootstrap 引入的 Modal -->
    <!-- 给以下的弹框设置一个自己的类名，方便上面的 button 按钮绑定 -->
    <div class="modal my-box" tabindex="-1">
        <div class="modal-dialog">
            <!-- 弹框 - 内容 -->
            <div class="modal-content">
                <!-- 弹框 - 头部 -->
                <div class="modal-header">
                    <h5 class="modal-title">Modal title</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <!-- 弹框 - 身体 -->
                <div class="modal-body">
                    <p>Modal body text goes here.</p>
                </div>
                <!-- 弹框 - 底部 -->
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary">Save changes</button>
                </div>
            </div>
        </div>
    </div>
</body>
```

### 通过 JS 控制

额外逻辑代码，增加功能：

1.  点击 “编辑姓名” 按钮弹出弹框，同时赋予输入框默认值
2.  点击 “保存” 按钮获取输入框数据提交给服务器

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230803113128.png)

```js
// 创建弹框对象
const modalDom = document.querySelector('css选择器');
const modal = new bootstrap.Modal(modelDom);
// 显示弹框
modal.show();
// 隐藏弹框
modal.hide();
```

```html
<head>
    <!-- 引入 bootstrap.css -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- 引入 bootstrap.js -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.min.js"></script>
</head>
<body>
    <button type="button" class="btn btn-primary edit-btn">编辑姓名</button>
    <!-- 以下是从 bootstrap 引入的 Modal -->
    <!-- 给以下的弹框设置一个自己的类名，方便上面的 button 按钮绑定 -->
    <div class="modal name-box" tabindex="-1">
        <div class="modal-dialog">
            <!-- 弹框 - 内容 -->
            <div class="modal-content">
                <!-- 弹框 - 头部 -->
                <div class="modal-header">
                    <h5 class="modal-title">请输入姓名</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <!-- 弹框 - 身体 -->
                <div class="modal-body">
                    <form action="">
                        <span>姓名：</span>
                        <input type="text" class="username">
                    </form>
                </div>
                <!-- 弹框 - 底部 -->
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">取消</button>
                    <button type="button" class="btn btn-primary save-btn">保存</button>
                </div>
            </div>
        </div>
    </div>
</body>
```

```js
// 1. 创建弹框对象
const modalDom = document.querySelector('.name-box');
const modal = new bootstrap.Modal(modelDom);

// 编辑姓名 -> 点击 -> 赋予默认姓名 -> 弹框显示
document.querySelector('.edit-btn').addEventListener('click', () => {
    document.querySelector('.username').value = '默认姓名';
    // 2. 显示弹框
    modal.show();
})

// 保存 -> 点击 -> 获取姓名 -> 弹框隐藏
document.querySelector('.save-btn').addEventListener('click', () => {
    const username = document.querySelector('.username').value;
    // 2. 隐藏弹框
    modal.hide();
})
```

## 图书管理 - 渲染列表

自己的图书数据：给自己起个外号，并告诉服务器，默认会有三本书，基于这三本书做数据的增删改查

获取数据 -> 渲染数据

```js
const creator = '老张'
// 封装 - 获取并渲染图书列表函数
function getBooksList() {
    axios({
        url: 'http://hmajax.itheima.net/api/books',
        params: {
            creator
        }
    }).then(result => {
        const bookList = reault.data.data;// 一个数组，每个元素是一个图书对象
        // 渲染数据
        const htmlStr = bookList.map((item, index) => {
            return `<tr>
      					<td>${index + 1}</td>
                      	<td>${item.bookname}</td>
                      	<td>${item.author}</td>
                      	<td>${item.publisher}</td>
                      	<td data-id=${item.id}>
                        	<span class="del">删除</span>
                        	<span class="edit">编辑</span>
                      	</td>
    				</tr>`
        }).join('');// 自定义属性 data-id，方便删除图书时判断删除的是哪个图书
        document.querySelector('.list').innerHTML = htmlStr;
    })
}
// 网页加载运行，获取并渲染列表一次
getBooksList()
```

## 图书管理 - 新增图书

新增图书 - 弹框（显示 & 隐藏） -> 收集数据 & 提交保存 -> 刷新 - 图书列表

<table><tbody><tr><td data-num="1"></td><td><pre><span class="token comment">// 创建弹框对象</span></pre></td></tr><tr><td data-num="2"></td><td><pre><span class="token keyword">const</span> addModalDom <span class="token operator">=</span> document<span class="token punctuation">.</span><span class="token function">querySelector</span><span class="token punctuation">(</span><span class="token string">'.add-modal'</span><span class="token punctuation">)</span><span class="token punctuation">;</span></pre></td></tr><tr><td data-num="3"></td><td><pre><span class="token keyword">const</span> addModal <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">bootstrap</span><span class="token punctuation">(</span>addModalDom<span class="token punctuation">)</span><span class="token punctuation">;</span></pre></td></tr><tr><td data-num="4"></td><td><pre><span class="token comment">// 保存按钮 -&gt; 点击 -&gt; 隐藏弹框</span></pre></td></tr><tr><td data-num="5"></td><td><pre>document<span class="token punctuation">.</span><span class="token function">querySelector</span><span class="token punctuation">(</span><span class="token string">'.add-btn'</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">addEventListener</span><span class="token punctuation">(</span><span class="token string">'click'</span><span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span></pre></td></tr><tr><td data-num="6"></td><td><pre>    <span class="token comment">// 收集表单数据，并提交到服务器保存</span></pre></td></tr><tr><td data-num="7"></td><td><pre>    <span class="token keyword">const</span> addForm <span class="token operator">=</span> document<span class="token punctuation">.</span><span class="token function">querySelector</span><span class="token punctuation">(</span><span class="token string">'.add-form'</span><span class="token punctuation">)</span><span class="token punctuation">;</span></pre></td></tr><tr><td data-num="8"></td><td><pre>    <span class="token keyword">const</span> bookObj <span class="token operator">=</span> <span class="token function">serialize</span><span class="token punctuation">(</span>addForm<span class="token punctuation">,</span> <span class="token punctuation">{</span><span class="token literal-property property">hash</span><span class="token operator">:</span> <span class="token boolean">true</span><span class="token punctuation">,</span> <span class="token literal-property property">empty</span><span class="token operator">:</span> <span class="token boolean">true</span><span class="token punctuation">}</span><span class="token punctuation">)</span></pre></td></tr><tr><td data-num="9"></td><td><pre>    <span class="token comment">// 提交到服务器</span></pre></td></tr><tr><td data-num="10"></td><td><pre>    <span class="token function">axios</span><span class="token punctuation">(</span><span class="token punctuation">{</span></pre></td></tr><tr><td data-num="11"></td><td><pre>        <span class="token literal-property property">url</span><span class="token operator">:</span> <span class="token string">'http://hmajax.itheima.net/api/books'</span><span class="token punctuation">,</span></pre></td></tr><tr><td data-num="12"></td><td><pre>        <span class="token literal-property property">method</span><span class="token operator">:</span> <span class="token string">'post'</span><span class="token punctuation">,</span></pre></td></tr><tr><td data-num="13"></td><td><pre>        <span class="token literal-property property">data</span><span class="token operator">:</span> <span class="token punctuation">{</span></pre></td></tr><tr><td data-num="14"></td><td><pre>            <span class="token operator">...</span>bookObj<span class="token punctuation">,</span></pre></td></tr><tr><td data-num="15"></td><td><pre>            creator</pre></td></tr><tr><td data-num="16"></td><td><pre>        <span class="token punctuation">}</span></pre></td></tr><tr><td data-num="17"></td><td><pre>    <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">then</span><span class="token punctuation">(</span><span class="token parameter">result</span> <span class="token operator">=&gt;</span> <span class="token punctuation">{</span></pre></td></tr><tr><td data-num="18"></td><td><pre>        <span class="token comment">// 添加成功后，重新请求并渲染图书列表</span></pre></td></tr><tr><td data-num="19"></td><td><pre>        <span class="token function">getBookList</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span></pre></td></tr><tr><td data-num="20"></td><td><pre>        <span class="token comment">// 重置表单，不写这行代码的话点保存按钮再点添加按钮会发现表单还填着已提交的内容</span></pre></td></tr><tr><td data-num="21"></td><td><pre>        addForm<span class="token punctuation">.</span><span class="token function">reset</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span></pre></td></tr><tr><td data-num="22"></td><td><pre>        <span class="token comment">// 隐藏弹框</span></pre></td></tr><tr><td data-num="23"></td><td><pre>        addModal<span class="token punctuation">.</span><span class="token function">hide</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span></pre></td></tr><tr><td data-num="24"></td><td><pre>    <span class="token punctuation">}</span><span class="token punctuation">)</span></pre></td></tr><tr><td data-num="25"></td><td><pre><span class="token punctuation">}</span><span class="token punctuation">)</span></pre></td></tr></tbody></table>

## 图书管理 - 删除图书

绑定点击事件（获取图书 id） -> 调用删除接口 -> 刷新图书列表

```js
// 删除元素 -> 点击（事件委托，委托给 tbody，就不需要给每个删除按钮循环绑定事件了）
document.querySelector('.list').addEventListener('click', e => {
    // 判断点击的是删除元素
    if (e.target.classList.contains('del')) {
        // 获取图书 id
        const theId = e.target.parentNode.dataset.id;
        // 调用删除接口
        axios({
            // 接口文档写的参数位置为 path
            url: `http://hmajax.itheima.net/api/books/${theId}`,
            method: 'delete'
        }).then(() => {
            getBooksList();
        })
    }
})
```

## 图书管理 - 编辑图书

编辑图书 - 弹框（显示 & 隐藏） -> 表单（数据回显） -> 保存修改 & 刷新列表

```js
const editDom = document.querySelector('.edit-modal');
const editModal = new bootstrap.Modal(editDom);
document.querySelector('.list').addEventListener('click', e => {
    if (e.target.classList.contains('edit')) {
        // 获取当前编辑图书数据 -> 回显到编辑表单中
        const theId = e.target.parentNode.dataset.id;
        axios({
            url: `http://hmajax.itheima.net/api/books/${theId}`
            // 默认 get 方法
        }).then(result => {
            const bookObj = result.data.data;
            /* 
            document.querySelector('.edit-form .bookname').value = bookObj.bookname;
            document.querySelector('.edit-form .author').value = bookObj.author; 
            */
            // 数据对象的属性和标签类名一致
            // 遍历数据对象，使用属性去获取对应的标签，快速取值
            const keys = Object.keys(bookObj);//keys 是一个数组，每个元素为 bookObj 属性名的字符串
            keys.forEach(key => {
                document.querySelector(`.edit-form .${key}`).value = bookObj[key];
            })
        })
        editModal.show();
    }
})
// 修改按钮 -> 点击 -> 隐藏弹框
document.querySelector('.edit-btn').addEventListener('click', () => {
    const editForm = document.querySelector('.edit-form');
    const {id, bookname, author, publisher} = serialize(editForm, {hash: true, empty: true});
    axios({
        url: `http://hmajax.itheima.net/api/books/${id}`,
        method: 'PUT',
        data: {
            bookname,
            author,
            pubisher,
            creator
        }
    }).then(() => {
        getBooksList();
    })
    editModal.hide();
})
```

# 图片上传

1. 获取图片文件对象

2. 使用 FormData 携带图片文件

   ```js
   const fd = new FormData()
   fd.append(参数名, 值)
   ```

3. 提交表单数据到服务器，使用图片 url 网址

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230807105911.png)

```html
<!-- 文件选择元素 -->
<input type="file" class="upload">
<img src="" alt="" class="my-img">
```

```js
// 监听文件选择元素选完图片之后的 change 改变事件
document.querySelector('.upload').addEventListener('change', e => {
    // 1. 获取图片文件
    //e.target 是触发事件的文件选择元素 & lt;input type="file" class="upload">
    console.log(e.target.files[0]);
    // 刚刚用户选的文件就在 inupt 元素里边，原生属性 files 可以拿到文件选择元素 input 里边的文件列表
    // 2. 使用 FormData 携带图片文件
    const fd = new FormData();
    fd.append('img', e.target.files[0]);
    // 3. 提交到服务器，获取图片 url 网址使用
    axios({
        url: 'http://hmajax.itheima.net/api/uploadimg',
        method: 'POST',
        data: fd
    }).then(result => {
        // 取出图片 url 网址，用 img 标签加载显示
        const imgUrl = result.data.data.url;
        document.querySelector('.my-img').src = imgUrl;
    })
})
```

# 案例 - 网站换肤

1.  选择图片上传，设置 body 背景
2.  上传成功时，保存 url 网址
3.  网页运行后，获取 url 网址使用

```html
<label for="bg">更换背景</label>
<input class="bg-ipt" type="file" id="bg">
```

```js
document.querySelector('.bg-ipt').addEventListener('change', e => {
    const fd = new FormData();
    fd.append('img', e.target.files[0])
    axios({
        url: 'http://hmajax.itheima.net/api/uploadimg',
        method: 'POST',
        data: fd
    }).then(result => {
        const imgUrl = result.data.data.url;
        document.body.backgroundImage = `url(${imgUrl})`;
        // 2. 上传成功时，“保存” 图片 url 网址
        localStorage.setItem('bgImg', imgUrl);
    })
})
// 3. 网页运行后，“获取” url 网址使用
const bgUrl = localStorage.getItem('bgImg');
bgUrl && document.body.style.backgroundImage = `url(${bgUrl})`;// 如果 bgUrl 值为空，则 & amp;& 后面的代码就不执行了
```

# 案例 - 个人信息设置

步骤：

1.  信息渲染
2.  头像修改
3.  提交表单
4.  结果提示

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230807124843.png)

## 信息渲染

自己的用户信息：给自己起个外号，并告诉服务器，获取对应的用户信息

获取数据 -> 渲染数据

```js
const creator = 'lxy';
// 1.1 获取用户的数据
aixos({
    url: 'http://hmajax.itheima.net/api/settings',
    params: {
        creator
    }
}).then(result => {
    const userObj = result.data.data;// {avator: ' 头像图片文件的网址 ', desc: ' 我是李欣怡，大家好！！！', email: 'lxy1@itcast.cn', gender: 1, nickname: 'lxy1'}
    // 1.2 回显数据到标签上
    Object.keys(userObj).forEach(key => {
        if (key === 'avator') {
            document.querySelector('.prew').src = userObj[key];
        }
        else if (key === 'gender') {
            // 获取性别单选框：[男 radio 元素，女 radio 元素]
            const gRadioList = document.querySelectorAll('.gender');
            // 获取性别数字：0 男，1 女
            const gNum = userObj[key];
            gRadioList[gNum].checked = true;
        }
        else {
            document.querySelector(`.${key}`).value = userObj[key];
        }
    })
})
```

## 头像修改

选择头像文件 -> 提交保存 + 回显

```html
<label for="upload">更换头像</label>
<input id="upload" type="file" class="upload">
```

```js
document.querySelector('.upload').addEventListener('change', e => {
    const fd = new FormData();
    fd.append('avator', e.target.files[0]);
    fd.append('creator', creator);
    axios({
        url: 'http://hmajax.itheima.net/api/avator',
        method: 'put',
        data: fd
    }).then(result => {
        const imgUrl = result.data.data.avator;
        // 把新的头像回显到页面上
        document.querySelector('.prew').src = imgUrl;
    })
})
```

## 信息修改

收集表单 -> 提交保存

```js
document.querySelector('.submit').addEventListener('click', () => {
    // 收集表单信息
    const userForm = document.querySelector('.user-form');
    const userObj = serialize(userForm, {hash: true, empty: true})
    userObj.creator = creator;
    // 性别数字字符串，转成数字类型
    userObj.gender = +userObj.gender;
    // 提交到服务器保存
    axios({
        url: 'http://hmajax.itheima.net/api/settings',
        method: 'put',
        data: userObj
    }).then(result => {// 这行开始为” 提示框 “代码
        const toastDom = document.querySelector('.my-toast');
        const toast = new bootstrap.Toast(toastDom);
        toast.show();
    })
})
```

## 提示框

确认 - 提示框 -> 控制显示

```html
<div class="toast my-toast" data-bs-delay="1500">
    <div class="toast-body">
        <div class="alert alert-success info-box">
            提示框内容
        </div>
    </div>
</div>
```

```js
// 创建提示框对象
const toastDom = document.querySelector('css选择器');
const toast = new bootstrap.Toast(toastDom);
// 显示提示框
toast.show();
```

在上一节 “信息修改” 中继续修改代码
