---
title: ECharts
date: 2022-04-22
cover: assets/echarts.png
categories:
- [前端]
tags:
  - 数据可视化
  - ECharts
  - 前端
---

# 什么是数据可视化

## 数据可视化

+   数据可视化主要目的：借助于图形化手段，清晰有效地传达与沟通信息。
+   数据可视化可以把数据从冰冷的数字转换成图形，揭示蕴含在数据中的规律和道理。

## 数据可视化的场景

目前互联网公司通常有这么几大类的可视化需求：

+   通用报表
+   移动端图表
+   大屏可视化
+   图编辑 & 图分析
+   地理可视化

## 常见的数据可视化库

+   D3.js，目前 Web 端评价最高的 Javascript 可视化工具库 (入手难)
+   ECharts.js。百度出品的一个开源 Javascript 数据可视化库
+   Highcharts.js，国外的前端数据可视化库，非商用免费，被许多国外大公司所使用
+   AntV，蚂蚁金服全新一代数据可视化解决方案

# ECharts 简介

ECharts 是一个使用 JavaScript 实现的开源可视化库，可以流畅的运行在 PC 和移动设备上，兼容当前绝大部分浏览器（IE8/9/10/11，Chrome，Firefox，Safari 等），底层依赖矢量图形库 ZRender，提供直观，交互丰富，可高度个性化定制的数据可视化图表。

官网地址：https://www.echartsjs.com/zh/index.html

+   丰富的可视化类型
+   多种数据格式支持
+   流数据的支持
+   移动端优化
+   跨平台使用
+   绚丽的特效详
+   细的文档说明

# Echarts 的基本使用

## ECharts 使用五步曲

1.  下载并引入 echarts.js 文件
    
    图表依赖这个 js 库
    
2.  准备一个具备大小的 DOM 容器
    
    生成的图表会放入这个容器内
    
3.  初始化 echarts 实例对象
    
    实例化 echarts 对象
    
4.  指定配置项和数据 (option)
    
    根据具体需求修改配置选项
    
5.  将配置项设置给 echarts 实例对象
    
    让 echarts 对象根据修改好的配置生效
    

## 相关配置讲解

+   title：标题组件
    
+   tooltip：提示框组件
    
+   legend：图例组件
    
+   toolbox: 工具栏
    
+   grid：直角坐标系内绘图网格
    
+   xAxis：直角坐标系 grid 中的 x 轴
    
+   yAxis：直角坐标系 grid 中的 y 轴
    
+   series: 系列列表
    
    +   type: 类型 (什么类型的图表) 比如 line 是折线 bar 柱形等
        
    +   name: 系列名称，用于 tooltip 的显示，legend 的图例筛选变化
        
    +   stack: 数据堆叠。如果设置相同值，则会数据堆叠。
        
        第二个数据值 = 第一个数据值 + 第二个数据值
        
        第三个数据值 = 第二个数据值 + 第三个数据值…. 依次叠加
        
        如果给 stack 指定不同值或者去掉这个属性则不会发生数据堆叠
    
+   color：调色盘颜色列表
    

其余配置还有具体细节查阅文档：文档菜单 — 配置项手册

学 echarts 关键在于学会查阅文档，根据需求修改配置

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230801231019.png)

```js
option = {
  title: {
    text: 'Stacked Line'
  },
  tooltip: {
    trigger: 'axis'
  },
  legend: {
    data: ['Email', 'Union Ads']
  },
  grid: {
    left: '3%',
    right: '4%',
    bottom: '3%',
    containLabel: true
  },
  toolbox: {
    feature: {
      saveAsImage: {}
    }
  },
  xAxis: {
    type: 'category',
    boundaryGap: false,
    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      name: 'Email',
      type: 'line',
      stack: 'Total',
      data: [120, 132, 101, 134, 90, 230, 210]
    },
    {
      name: 'Union Ads',
      type: 'line',
      stack: 'Total',
      data: [220, 182, 191, 234, 290, 330, 310]
    },
  ]
};
```

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230801231158.png)
