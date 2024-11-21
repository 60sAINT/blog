---
title: JS内置对象 Object 详解
date: 2023-08-07
cover: assets/jsObject.png
categories:
- [前端, JavaScript]
tags:
  - JavaScript
---

# 方法

## `Object.keys()`

**`Object.keys()`** 静态方法返回一个由给定对象自身的可枚举的字符串键属性名组成的数组。

+   尝试一下
    
    ```js
    const object1 = {
      a: 'somestring',
      b: 42,
      c: false,
    };
    console.log(Object.keys(object1));
    // Expected output: Array ["a", "b", "c"]
    ```
    
+   语法
    
    ```js
    Object.keys(obj)
    ```
    
    +   参数：
        
        +   `obj`
            
            一个对象
        
    +   返回值：
        
        一个由给定对象自身可枚举的字符串键属性键组成的数组。
    
+   示例
    
    > 📗使用 `Object.keys()`
    >
    > ```js
    > // 简单数组
    > const arr = ["a", "b", "c"];
    > console.log(Object.keys(arr)); // ['0', '1', '2']
    > // 类数组对象
    > const obj = { 0: "a", 1: "b", 2: "c" };
    > console.log(Object.keys(obj)); // ['0', '1', '2']
    > // 键的顺序随机的类数组对象
    > const anObj = { 100: "a", 2: "b", 7: "c" };
    > console.log(Object.keys(anObj)); // ['2', '7', '100']
    > ```
    
    > 🍏在基本类型中使用 `Object.keys()`
    >
    > 非对象参数会强制转换为对象。只有字符串可以有自己的可枚举属性，而其他所有基本类型都返回一个空数组。
    >
    > ```js
    > // 字符串具有索引作为可枚举的自有属性
    > console.log(Object.keys("foo")); // ['0', '1', '2']
    > // 其他基本类型没有自有属性
    > console.log(Object.keys(100)); // []
    > ```
