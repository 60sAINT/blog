---
title: AJAX - 进阶篇
date: 2023-08-29
cover: assets/AJAX.png
categories:
- [前端, AJAX]
tags:
  - JavaScript
  - 前端
  - AJAX
---

# 同步代码和异步代码

+   <u>同步代码</u>：逐行执行，需原地等待结果后，才继续向下执行
    
    我们应该注意的是，实际上浏览器是按照我们<u>书写代码的顺序一行一行地执行程序的</u>。浏览器会等待代码的解析和工作，在<u>上一行完成后才会执行下一行</u>。这样做是很有必要的，因为每一行新的代码都是建立在前面代码的基础之上的
    
    这也使得它成为一个同步程序
    
+   <u>异步代码</u>：调用后耗时，不阻塞代码继续执行（不必原地等待），在将来完成后触发一个回调函数
    
    异步编程技术使你的程序可以在执行一个<u>可能长期运行的任务</u>的同时继续对其他事件做出反应<u>不必等待任务完成</u>。与此同时，你的程序也将在任务<u>完成后显示结果</u>
    

> 例子：回答打印数字的顺序是什么？
>
> ```js
> const result = 0 + 1;
> console.log(result);
> setTimeout(() => {
>     console.log(2)
> }, 2000);
> document.querySelector('.btn').addEventListener('click', () => {
>     console.log(3)
> })
> document.body.style.backgroundColor = 'pink';
> console.log(4);
> ```
>
> 打印结果：1,4,2
>
> 点击按钮一次就打印一次 3
>
> 异步代码接收结果：使用回调函数

# 回调函数地狱

需求：展示默认第一个省，第一个城市，第一个地区在下拉菜单中

```js
// 1. 获取默认第一个省份的名字
    axios({url: 'http://hmajax.itheima.net/api/province'}).then(result => {
      const pname = result.data.list[0]
      document.querySelector('.province').innerHTML = pname
      // 2. 获取默认第一个城市的名字
      axios({url: 'http://hmajax.itheima.net/api/city', params: { pname }}).then(result => {
        const cname = result.data.list[0]
        document.querySelector('.city').innerHTML = cname
        // 3. 获取默认第一个地区的名字
        axios({url: 'http://hmajax.itheima.net/api/area', params: { pname, cname }}).then(result => {
          console.log(result)
          const areaName = result.data.list[0]
          document.querySelector('.area').innerHTML = areaName
        })
      })
    }).catch(error => {
      console.dir(error)// 若第三步获取默认第一个地区的名字传入 url 错误，该行不会执行，错误直接被 axios 内部代码捕获抛出到浏览器中
    })
```

概念：在回调函数中嵌套回调函数，一直嵌套下去就形成了回调函数地狱

缺点：可读性差，异常无法捕获，耦合性严重，牵一发动全身

# Promise - 链式调用

概念：依靠 then () 方法会返回一个新生成的 Promise 对象特性，继续串联下一环任务，直到结束

细节：then () 回调函数中的返回值，会影响新生成的 Promise 对象最终状态和结果

好处：通过链式调用，解决回调函数嵌套问题

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230829134953.png)

```js
/**
     * 目标：掌握 Promise 的链式调用
     * 需求：把省市的嵌套结构，改成链式调用的线性结构
    */
    // 1. 创建 Promise 对象 - 模拟请求省份名字
    const p = new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('北京市')
      }, 2000)
    })

    // 2. 获取省份名字
    const p2 = p.then(result => {
      console.log(result)
      // 3. 创建 Promise 对象 - 模拟请求城市名字
      //return Promise 对象最终状态和结果，影响到新的 Promise 对象
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(result + '--- 北京')
        }, 2000)
      })
    })

    // 4. 获取城市名字
    p2.then(result => {
      console.log(result)
    })

    //then () 返回的结果是一个新的 Promise 对象
    console.log(p2 === p)
```

> 📗Promise 链式应用
>
> 目标：使用 Promise 链式调用，解决回调函数地狱问题
>
> 做法：每个 Promise 对象中管理一个异步任务，用 then 返回 Promise 对象，串联起来
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230829144835.png)
>
> ```js
> /**
>     * 目标：把回调函数嵌套代码，改成 Promise 链式调用结构
>     * 需求：获取默认第一个省，第一个市，第一个地区并展示在下拉菜单中
>    */
>    let pname = ''
>    // 1. 得到 - 获取省份 Promise 对象
>    axios({url: 'http://hmajax.itheima.net/api/province'}).then(result => {
>      pname = result.data.list[0]
>      document.querySelector('.province').innerHTML = pname
>      // 2. 得到 - 获取城市 Promise 对象
>      return axios({url: 'http://hmajax.itheima.net/api/city', params: { pname }})
>    }).then(result => {
>      const cname = result.data.list[0]
>      document.querySelector('.city').innerHTML = cname
>      // 3. 得到 - 获取地区 Promise 对象
>      return axios({url: 'http://hmajax.itheima.net/api/area', params: { pname, cname }})
>    }).then(result => {
>      console.log(result)
>      const areaName = result.data.list[0]
>      document.querySelector('.area').innerHTML = areaName
>    })
> ```

# async 和 await 使用

<u>定义</u>：async 函数是使用 `async` 关键字声明的函数。async 函数是 <u>`AsyncFunction`</u> 构造函数的实例，并且其中允许使用 `await` 关键字。 `async` 和 `await` 关键字让我们可以用一种更简洁的方式写出基于 <u>`Promise`</u> 的异步行为，而无需刻意地链式调用 `promise`

概念： 在 async 函数内，使用 await 关键字取代 then 函数，等待获取 Promise 对象成功状态的结果值

示例：

```js
// 1. 定义 async 修饰函数
    async function getData() {
      // 2. await 等待 Promise 对象成功的结果
      const pObj = await axios({url: 'http://hmajax.itheima.net/api/province'})
      const pname = pObj.data.list[0]
      const cObj = await axios({url: 'http://hmajax.itheima.net/api/city', params: { pname }})
      const cname = cObj.data.list[0]
      const aObj = await axios({url: 'http://hmajax.itheima.net/api/area', params: { pname, cname }})
      const areaName = aObj.data.list[0]


      document.querySelector('.province').innerHTML = pname
      document.querySelector('.city').innerHTML = cname
      document.querySelector('.area').innerHTML = areaName
    }

    getData()
```

