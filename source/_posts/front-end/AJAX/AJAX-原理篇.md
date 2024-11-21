---
title: AJAX - 原理篇
date: 2023-08-24
cover: assets/AJAX.png
categories:
- [前端, AJAX]
tags:
  - JavaScript
  - 前端
  - AJAX
---

# XMLHttpRequest

+   定义： `XMLHttpRequest` （XHR）对象用于与服务器交互。通过 XMLHttpRequest 可以在不刷新页面的情况下请求特定 URL，获取数据。这允许网页在不影响用户操作的情况下，更新页面的局部内容。 `XMLHttpRequest` 在AJAX编程中被大量使用。
    
+   关系：axios 内部采用 XMLHttpRequest 与服务器交互
    
    ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230824155906.png)
    
+ 好处：掌握使用 XHR 与服务器进行数据交互，了解 axios 内部原理


## 使用 XMLHttpRequest

步骤：

1.  创建 XMLHttpRequest 对象
2.  配置请求方法和请求 url 地址
3.  监听 loadend 事件，接收响应结果
4.  发起请求

```js
const xhr = new XMLHttpRequest();
xhr.open('请求方法', '请求url网址');
xhr.addEventListener('loadend', () => {
    // 响应结果
    console.log(xhr.response);//response 是 xhr 对象里的一个固定属性
})
xhr.send();// 这句代码执行后才会真正发起一次请求到服务器
```

> 📗需求：获取并展示所有省份名字
>
> ```html
> <p class="my-p"></p>
> ```
>
> ```js
> const xhr = new XMLHttpRequest();
> xhr.open('GET', 'http://hmajax.itheima.net/api/province');
> xhr.addEventListener('loadend', () => {
>    //xhr.response 是一个对象结构的 json 字符串，而 axios 会把返回的数据从 json 字符串转成 js 对象
>    const data = JSON.parse(xhr.response);
>    document.querySelector('.my-p').innerHTML = data.list.join('<br>');
> })
> xhr.send();
> ```

## 查询参数

定义：浏览器提供给服务器的额外信息，让服务器返回浏览器想要的数据

语法：http://xxxx.com/xxx/xxx? 参数名 1 = 值 1 & 参数名 2 = 值 2

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230828111629.png)

```html
<p class="city-p"></p>
```

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', 'http://hmajax.itheima.net/api/city?pname=河北省');
xhr.addEventListener('loadend', () => {
    const data = JSON.parse(xhr.response);
    document.querySelector('.city-p').innerHTML = data.list.join('<br>');
});
xhr.send();
```

> 📗案例：地区查询
>
> 需求：输入省份和城市名字，查询地区列表
>
> 请求地址：http://hmajax.itheima.net/api/area? 参数名 1 = 值 1 & 参数名 2 = 值 2
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230828112517.png)
>
> ```js
> // URLSearchParams 对象：把普通的 js 对象转成查询参数字符串的格式
> // 1. 创建 URLSearchParams 对象
> const paramsObj = new URLSearchParams({
>   参数名1: 值1,
>   参数名2: 值2
> })
> // 2. 生成指定格式查询参数字符串
> const queryString = paramsObj.toString();// 结果：参数名 1 = 值 1& 参数名 2 = 值 2
> ```
>
> ```js
> // 1. 查询按钮 - 点击事件
> document.querySelector('.sel-btn').addEventListener('click', () => {
>    // 2. 收集省份和城市名字
>    const pname = document.querySelector('.province').value;
>    const cname = document.querySelector('city').value;
>    // 3. 组织查询参数字符串
>    const qObj = {
>        pname,
>        cname
>    }
>    // 查询参数对象 -> 查询参数字符串
>    const paramsObj = new URLSearchParams(qObj);
>    const queryString = paramsObj.toString();
>    // 4. 使用 xhr 对象，查询地区列表
>    const xhr = new XMLHttpRequest();
>    xhr.open('GET', `http://hmajax.itheima.net/api/area?${queryString}`);
>    xhr.addEventListener('loadend', () => {
>        const data = JSON.parse(xhr.response);
>        const htmlStr = data.list.map(areaName => {
>            return `<li class="list-group-item">${areaName}</li>`
>        }).join('')
>        document.querySelector('.list-group').innerHTML = htmlStr;
>    });
>    xhr.send();
> })
> ```

## 数据提交

+   需求：通过 XHR 提交用户名和密码，完成注册功能
    
    ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230828195705.png)
    
+ 核心：

  请求头设置 Content-Type：application/json

  请求体携带 JSON 字符串

  ```js
  const xhr = new XMLHttpRequest();
  xhr.open('请求方法', '请求url网址');
  xhr.addEventListener('loadend', () => {
      console.log(xhr.response);
  })
  // 告诉服务器我传递的内容是 JSON 字符串
  xhr.setRequestHeader('Content-Type', 'application/json');
  // 准备数据并转成 JSON 字符串
  const user = {username: 'lxy0819', password: '123456'};
  const userStr = JSON.stringfy(user);
  // 发送请求体数据
  xhr.send(userStr);
  ```

```html
<button class="reg-btn">注册用户</button>
```

```js
document.querySelector('.reg-btn').assEventListener('click', () => {
    const xhr = new XMLHttpRequest();
	xhr.open('POST', 'http://hmajax.itheima.net/api/register');
    xhr.addEventListener('loadend', () => {
        console.log(xhr.response);
    });
    xhr.setRequestHeader('Content-Type', 'application/json');
    // 准备提交的数据
    const userObj = {
        username: 'lxy0819', 
        password: '123456'
    };
    const userStr = JSON.stringfy(userObj);
    // 设置请求体，发起请求
    xhr.send(userStr);
})
```

# Promise

+   定义： `Promise` 对象用于表示一个异步操作的最终完成（或失败）及其结果值
    
+   好处：
    
    1.  逻辑更清晰
    2.  了解 axios 函数内部运作机制
    3.  能解决回调函数地狱问题
+   语法：
    
    ```js
    // 1. 创建 Promise 对象
    const p = new Promise((resolve, reject) => {// 这两个形参为函数
        // 2. 执行异步任务 - 并传递结果
        // 成功调用：resolve (值) 触发 then () 执行
        // 失败调用：reject (值) 触发 catch () 执行
    })
    // 3. 接收结果
    p.then(result => {
    	// 成功
    }).catch(error => {
    	// 失败
    })
    ```
    
+   三种状态：
    
    一个 Promise 对象，必然处于以下几种状态之一
    
    +   待定（pending）：初始状态，既没有被兑现，也没有被拒绝
    +   已兑现（fulfilled）：意味着，操作成功完成
    +   已拒绝（rejected）：意味着，操作失败
    
    注意：Promise 对象一旦被兑现 / 拒绝就是已敲定了，状态无法再被改变
    
    ```js
    // 1. 创建 Promise 对象（pending - 待定状态）
    const p = new Promise((resolve, reject) => {
        // Promise 对象创建时，这里的代码就会立刻执行
        // 2. 执行异步代码
        // 在异步函数执行之前，会先继续执行下面的代码，给 Promise 对象绑定 then 和 catch 的回调
        setTimeout(() => {
            //resolve () => 'fulfilled 状态 - 已兑现 ' => then ()
            resolve('模拟AJAX请求-成功结果');// 2s 后调用 resolve 后会把 Promise 对象的状态从 pending 改为 fulfilled
            //reject () => 'rejected 状态 - 已拒绝 ' => catch ()
            reject(new Error('模拟AJAX请求-失败结果'));// 两个函数都写，Promise 对象最后还是” 已兑现 “状态，因为先执行上面的代码。Promise 对象一旦被兑现 / 拒绝就是已敲定了，状态无法再被改变
        }, 2000)
    })
    // 3. 获取结果
    p.then(result => {
    	console.log(result);// 模拟 AJAX 请求 - 成功结果
    }).catch(error => {
    	console.log(error);// Error: 模拟 AJAX 请求 - 失败结果
    })
    ```

> 📗案例：使用 Promise + XHR 获取省份列表
>
> 需求：使用 Promise 管理 XHR 获取省份列表，并展示到页面上
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230828211527.png)
>
> 步骤：
>
> 1.  创建 Promise 对象
> 2.  执行 XHR 异步代码，获取省份列表
> 3.  关联成功或失败函数，做后续处理
>
> ```js
> // 1. 创建 Promise 对象
> const p = new Promise((resolve, reject) => {
>    // 2. 执行 xhr 异步代码，获取省份列表
>    const xhr = new XMLHttpRequest();
>    xhr.open('GET', 'http://hmajax.itheima.net/api/province');
>    xhr.addEventListener('loadend', () => {
>        //xhr 如何判断响应成功还是失败？——2 开头的都是成功响应状态码
>        if (xhr.status >=200 && xhr.status < 300) {
>            resolve(JSON.parse(xhr.response));
>        }
>        else {
>            reject(new Error(xhr.response));
>        }
>    })
> })
> // 3. 关联成功或失败函数，做后续处理
> p.then(result => {
>    document.querySelector('.my-p').innerHTML = result.list.join('<br>');
> }).catch(error => {
>    // 错误对象要用 console.dir 详细打印
>    console.dir(error);
>    // 服务器返回错误提示消息，插入到 p 标签显示
>    document.querySelector('.my-p').innerHTML = error.message;
> })
> ```

# 封装简易版 axios

## 获取省份列表

需求：基于 Promise + XHR 封装 myAxios 函数，获取省份列表展示

步骤：

1.  定义 myAxios 函数，接收配置对象，返回 Promise 对象
2.  发起 XHR 请求，默认请求方法为 GET
3.  调用成功 / 失败的处理程序
4.  使用 myAxios 函数，获取省份列表展示

```js
function myAxios(config) {
    return new Promise((resolve, reject) => {
        // XHR 请求
        // 调用成功 / 失败的处理程序
    })
}
myAxios({
    url: '目标资源地址'
}).then(result => {
    
}).catch(error => {
    
})
```

```html
<p class="my-p"></p>
```

```js
// 1. 定义 myAxios 函数，接收配置对象，返回 Promise 对象
    function myAxios(config) {
      return new Promise((resolve, reject) => {
        // 2. 发起 XHR 请求，默认请求方法为 GET
        const xhr = new XMLHttpRequest()
        xhr.open(config.method || 'GET', config.url)
        xhr.addEventListener('loadend', () => {
          // 3. 调用成功 / 失败的处理程序
          if (xhr.status >= 200 && xhr.status < 300) {
            resolve(JSON.parse(xhr.response))
          } else {
            reject(new Error(xhr.response))
          }
        })
        xhr.send()
      })
    }
// 4. 使用 myAxios 函数，获取省份列表展示
myAxios({
  url: 'http://hmajax.itheima.net/api/province'
}).then(result => {
  console.log(result)
  document.querySelector('.my-p').innerHTML = result.list.join('<br>')
}).catch(error => {
  console.log(error)
  document.querySelector('.my-p').innerHTML = error.message
})
```

## 获取地区列表

需求：修改 myAxios 函数支持传递查询参数，获取 "辽宁省"，"大连市" 对应地区列表展示

步骤：

1.  myAxios 函数调用后，判断 params 选项
2.  基于 URLSearchParams 转换查询参数字符串
3.  使用自己封装的 myAxios 函数展示地区列表

```js
function myAxios(config) {
    return new Promise((resolve, reject) => {
        // XHR 请求
        // 调用成功 / 失败的处理程序
    })
}
myAxios({
    url: '目标资源地址',
    params: {
        参数名1: 值1,
        参数名2: 值2
    }
}).then(result => {
    
}).catch(error => {
    
})
```

```html
<p class="my-p"></p>
```

```js
function myAxios(config) {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest()
        // 1. 判断有 params 选项，携带查询参数
        if (config.params) {
          // 2. 使用 URLSearchParams 转换，并携带到 url 上
          const paramsObj = new URLSearchParams(config.params)
          const queryString = paramsObj.toString()
          // 把查询参数字符串，拼接在 url？后面
          config.url += `?${queryString}`
        }

        xhr.open(config.method || 'GET', config.url)
        xhr.addEventListener('loadend', () => {
          if (xhr.status >= 200 && xhr.status < 300) {
            resolve(JSON.parse(xhr.response))
          } else {
            reject(new Error(xhr.response))
          }
        })
        xhr.send()
      })
    }

    // 3. 使用 myAxios 函数，获取地区列表
    myAxios({
      url: 'http://hmajax.itheima.net/api/area',
      params: {
        pname: '辽宁省',
        cname: '大连市'
      }
    }).then(result => {
      console.log(result)
      document.querySelector('.my-p').innerHTML = result.list.join('<br>')
    })
```

## 注册用户

需求：修改 myAxios 函数支持传递请求体数据，完成注册用户功能

步骤：

1.  myAxios 函数调用后，判断 data 选项
2.  转换数据类型，在 send 方法中发送
3.  使用自己封装的 myAxios 函数完成注册用户功能

```js
function myAxios(config) {
    return new Promise((resolve, reject) => {
        // XHR 请求
        // 调用成功 / 失败的处理程序
    })
}
myAxios({
    url: '目标资源地址',
    data: {
        参数名1: 值1,
        参数名2: 值2
    }
}).then(result => {
    
}).catch(error => {
    
})
```

```html
<button class="reg-btn">注册用户</button>
```

```js
function myAxios(config) {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest()

        if (config.params) {
          const paramsObj = new URLSearchParams(config.params)
          const queryString = paramsObj.toString()
          config.url += `?${queryString}`
        }
        xhr.open(config.method || 'GET', config.url)

        xhr.addEventListener('loadend', () => {
          if (xhr.status >= 200 && xhr.status < 300) {
            resolve(JSON.parse(xhr.response))
          } else {
            reject(new Error(xhr.response))
          }
        })
        // 1. 判断有 data 选项，携带请求体
        if (config.data) {
          // 2. 转换数据类型，在 send 中发送
          const jsonStr = JSON.stringify(config.data)
          xhr.setRequestHeader('Content-Type', 'application/json')
          xhr.send(jsonStr)
        } else {
          // 如果没有请求体数据，正常的发起请求
          xhr.send()
        }
      })
    }
  
    document.querySelector('.reg-btn').addEventListener('click', () => {
      // 3. 使用 myAxios 函数，完成注册用户
      myAxios({
        url: 'http://hmajax.itheima.net/api/register',
        method: 'POST',
        data: {
          username: 'itheima999',
          password: '666666'
        }
      }).then(result => {
        console.log(result)
      }).catch(error => {
        console.dir(error)
      })
    })
```

# 案例 - 天气预报

步骤：

1.  获取北京市天气数据，展示
2.  搜索城市列表，展示
3.  点击城市，显示对应天气数据

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230829105444.png)

```js
/**
 * 目标 1：默认显示 - 北京市天气
 *  1.1 获取北京市天气数据
 *  1.2 数据展示到页面
 */
// 获取并渲染城市天气函数
function getWeather(cityCode) {
  // 1.1 获取北京市天气数据
  myAxios({
    url: 'http://hmajax.itheima.net/api/weather',
    params: {
      city: cityCode
    }
  }).then(result => {
    console.log(result)
    const wObj = result.data
    // 1.2 数据展示到页面
    // 阳历和农历日期
    const dateStr = `<span class="dateShort">${wObj.date}</span>
    <span class="calendar">农历&nbsp;
      <span class="dateLunar">${wObj.dateLunar}</span>
    </span>`
    document.querySelector('.title').innerHTML = dateStr
    // 城市名字
    document.querySelector('.area').innerHTML = wObj.area
    // 当天气温
    const nowWStr = `<div class="tem-box">
    <span class="temp">
      <span class="temperature">${wObj.temperature}</span>
      <span>°</span>
    </span>
  </div>
  <div class="climate-box">
    <div class="air">
      <span class="psPm25">${wObj.psPm25}</span>
      <span class="psPm25Level">${wObj.psPm25Level}</span>
    </div>
    <ul class="weather-list">
      <li>
        <img src="${wObj.weatherImg}" class="weatherImg" alt="">
        <span class="weather">${wObj.weather}</span>
      </li>
      <li class="windDirection">${wObj.windDirection}</li>
      <li class="windPower">${wObj.windPower}</li>
    </ul>
  </div>`
    document.querySelector('.weather-box').innerHTML = nowWStr
    // 当天天气
    const twObj = wObj.todayWeather
    const todayWStr = `<div class="range-box">
    <span>今天：</span>
    <span class="range">
      <span class="weather">${twObj.weather}</span>
      <span class="temNight">${twObj.temNight}</span>
      <span>-</span>
      <span class="temDay">${twObj.temDay}</span>
      <span>℃</span>
    </span>
  </div>
  <ul class="sun-list">
    <li>
      <span>紫外线</span>
      <span class="ultraviolet">${twObj.ultraviolet}</span>
    </li>
    <li>
      <span>湿度</span>
      <span class="humidity">${twObj.humidity}</span>%
    </li>
    <li>
      <span>日出</span>
      <span class="sunriseTime">${twObj.sunriseTime}</span>
    </li>
    <li>
      <span>日落</span>
      <span class="sunsetTime">${twObj.sunsetTime}</span>
    </li>
  </ul>`
    document.querySelector('.today-weather').innerHTML = todayWStr

    // 7 日天气预报数据展示
    const dayForecast = wObj.dayForecast
    const dayForecastStr = dayForecast.map(item => {
      return `<li class="item">
      <div class="date-box">
        <span class="dateFormat">${item.dateFormat}</span>
        <span class="date">${item.date}</span>
      </div>
      <img src="${item.weatherImg}" alt="" class="weatherImg">
      <span class="weather">${item.weather}</span>
      <div class="temp">
        <span class="temNight">${item.temNight}</span>-
        <span class="temDay">${item.temDay}</span>
        <span>℃</span>
      </div>
      <div class="wind">
        <span class="windDirection">${item.windDirection}</span>
        <span class="windPower">${item.windPower}</span>
      </div>
    </li>`
    }).join('')
    document.querySelector('.week-wrap').innerHTML = dayForecastStr
  })
}

// 默认进入网页 - 就要获取天气数据（北京市城市编码：'110100'）
getWeather('110100')

/**
 * 目标 2：搜索城市列表
 *  2.1 绑定 input 事件，获取关键字
 *  2.2 获取展示城市列表数据
 */
// 2.1 绑定 input 事件，获取关键字。当一个 <input>, <select>, 或 <textarea> 元素的 value 被修改时，会触发 input 事件
document.querySelector('.search-city').addEventListener('input', (e) => {
  // 2.2 获取展示城市列表数据
  myAxios({
    url: 'http://hmajax.itheima.net/api/weather/city',
    params: {
      city: e.target.value
    }
  }).then(result => {
    console.log(result)
    const liStr = result.data.map(item => {
      // 给 li 设置自定义属性是为了点击 li 时获取该 li 的城市编码
      return `<li class="city-item" data-code="${item.code}">${item.name}</li>`
    }).join('')
    document.querySelector('.search-list').innerHTML = liStr
  })
})

/**
 * 目标 3：切换城市天气
 *  3.1 绑定城市点击事件，获取城市 code 值
 *  3.2 调用获取并展示天气的函数
 */
// 3.1 绑定城市点击事件，获取城市 code 值
document.querySelector('.search-list').addEventListener('click', e => {
  if (e.target.classList.contains('city-item')) {
    // 只有点击城市 li 才会走这里
    const cityCode = e.target.dataset.code
    // 3.2 调用获取并展示天气的函数
    getWeather(cityCode)
  }
})
```

