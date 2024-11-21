---
title: Postman HTTP 接口测试
date: 2023-04-15
categories:
- [软件测试与质量保证]
tags:
  - 软件测试
  - Postman
cover: assets/postman.png
---

# Postman 简介

Postman 是一类 HTTP 协议的 API 接口测试工具

# 下载安装 Postman

官网主页：https://www.postman.com/downloads/，下载所需版本进行安装即可

# 使用 Postman

+   点击 “Overview” 右边的加号创建新请求
    
    ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230416013413.png)
    
+ 各个模块功能：

  ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230416015422.png)


## 处理请求

1. 从浏览器顶部搜索引擎获取待测试接口的 URL

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230416193204.png)

2. 选择 HTTP 请求方式（这里为 “GET”），在 URL 区域输入链接

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230416193007.png)

3. 填入参数键值对

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230416193619.png)

4. 在 “Tests” 区域编写脚本

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230416194849.png)

   常见断言：

   +   校验接口响应的状态码，常见的有 200、401、404、500 等
       
       ```javascript
       pm.test("Status code is 200", function () {
           pm.response.to.have.status(200);
       });
       ```
       
   +   检查响应信息中是否包含某些指定的字符串
       
       ```javascript
       pm.test("Body matches string", function () {
           pm.expect(pm.response.text()).to.include("string_you_want_to_search");
       });
       ```
       
   +   检查从 JSON 响应中获取到某个字段，判断其是否与预期字段一致
       
       ```js
       pm.test("Your test name", function () {
           var jsonData = pm.response.json();
           pm.expect(jsonData.value).to.eql(100);
       });
       ```
       
   +   检查实际获取的响应体（即 Body 信息）与预期结果的响应体是否一致
       
       ```js
       pm.test("Body is correct", function () {
           pm.response.to.have.body("response_body_string");
       });
       ```
       
   +   检查响应中头域信息（Headers）的 Key 是否包含预期字段
       
       ```js
       pm.test("Content-Type is present", function () {
           pm.response.to.have.header("Content-Type");
       });
       ```
       
   +   判断实际响应时间是否低于预期时间
       
       ```js
       pm.test("Response time is less than 200ms", function () {
           pm.expect(pm.response.responseTime).to.be.below(200);
       });
       ```
       
   +   检查响应码是否与预期集合中的某个值一致
       
       ```js
       pm.test("Successful POST request", function () {
           pm.expect(pm.response.code).to.be.oneOf([201,202]);
       });
       ```
       
   +   检查响应状态原因是否为某个预期值
       
       ```js
       pm.test("Status code name has string", function () {
           pm.response.to.have.status("Created");
       });
       ```
       
   +   转化 XML 格式的响应成 JSON 对象
       
       ```js
       var jsonObject = xml2Json(responseBody);
       ```

5. 点击”Send“按钮


