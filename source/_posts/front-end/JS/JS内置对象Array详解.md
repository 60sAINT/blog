---
title: JS内置对象 Array 详解
date: 2023-08-02
cover: assets/jsArray.png
categories:
- [前端, JavaScript]
tags:
  - JavaScript
---

# 方法

## `Array.prototype.map()`

**`map()`** 方法**创建一个新数组**，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成。

+   尝试一下
    
    ```js
    const array1 = [1, 4, 9, 16];
    // Pass a function to map
    const map1 = array1.map(x => x * 2);
    console.log(map1);
    // Expected output: Array [2, 8, 18, 32]
    ```
    
+   语法
    
    ```js
    map(callbackFn)
    map(callbackFn, thisArg)
    ```
    
    +   参数：
        
        +   `callbackFn`
            
            为数组中的每个元素执行的函数。它的返回值作为一个元素被添加为新数组中。该函数被调用时将传入以下参数：
            
            +   `element`
                
                数组中当前正在处理的元素
                
            +   `index`
                
                正在处理的元素在数组中的索引
        
    +   返回值：
        
        一个新数组，每个元素都是回调函数的返回值。
    
+   示例
    
    > 📗求数组中每个元素的平方根
    >
    > ```js
    > // 下面的代码创建了一个新数组，值为原数组中对应数字的平方根。
    > const numbers = [1, 4, 9];
    > const roots = numbers.map((num) => Math.sqrt(num));
    > //roots 现在是     [1, 2, 3]
    > //numbers 依旧是   [1, 4, 9]
    > ```
    
    > 🍏在稀疏数组上使用 map ()
    >
    > 稀疏数组在使用 `map()` 方法后仍然是稀疏的。 `map()` 方法会跳过稀疏数组中未定义的元素，直接应用于已定义的元素。在新数组中，未定义的索引位置仍然保持为空缺。
    >
    > ```js
    > console.log(
    >  [1, , 3].map((x, index) => {
    >    console.log(`Visit ${index}`);
    >    return x * 2;
    >  }),
    > );
    > // Visit 0
    > // Visit 2
    > // [2, empty, 6]
    > ```
    
    > 💚映射包含 undefined 的数组
    >
    > ```js
    > // 当返回 undefined 或没有返回任何内容时：
    > const numbers = [1, 2, 3, 4];
    > const filteredNumbers = numbers.map((num, index) => {
    >  if (index < 3) {
    >    return num;
    >  }
    > });
    > //index 从 0 开始，因此 filterNumbers 为 1、2、3 和 undefined。
    > //filteredNumbers 是 [1, 2, 3, undefined]
    > //numbers 依旧是 [1, 2, 3, 4]
    > ```
