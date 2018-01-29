---
title: CSS 最佳实践 + 套路（六） -- flex
date: 2018-01-27 08:00:35
tags: CSS
---
# 概述
**CSS 弹性盒子（Flexible box 或 Flexbox）**是一种用于在页面上布置元素的布局模式，它的目的是允许容器可以让其子项目（子元素）能够改变宽度、高度、顺序等等，以最佳的方式填充空间（父元素）

**弹性盒布局**是指通过调整其内元素的宽高，从而在任何显示设备上实现对可用的显示空间最佳的填充能力

# 特点
- 与方向无关
- 实现空间的自动分配，自动对齐
- 适用于简单的线性布局

# 基本概念
![flexbox 基本概念](http://upload-images.jianshu.io/upload_images/9617841-c483db96d0c1e6bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 弹性容器（Flex container）
包含这弹性项目的父元素，通过设置 ` display: flex | inline-flex ` 定义弹性容器

### 弹性项目（Flex item）
弹性容器中的每个子元素都被称为弹性项目

### 轴（Axis）
每个弹性框包含两个轴。弹性项目延其依次排列的那根轴称为**主轴（main axis）**，垂直于主轴的那根轴称为**侧轴（cross axis）**

### 方向（direction）

***********************************************************************************
**很重要，知道方向才能了解弹性容器上的属性**
***********************************************************************************

**主轴方向**：弹性容器的**主轴起点（main start）**到**主轴终点（main end）**的方向
**侧轴方向**：弹性容器的**侧轴起点（cross start）**到**侧轴终点（cross end）**的方向

# 弹性容器属性

### 定义弹性容器
```
display: flex;            // 弹性容器是块级元素
或者
display: inline-flex;  // 弹性容器是行内元素
```
### flex-direction
确立**主轴**方向，可以改变 main start 到 main end 的方向

值:
   - ` flex-direction: row ` ==> 横向排列弹性项目，方向与内容方向相同（**main start 到 main end 的方向与内容方向相同**）

   - ` flex-direction: row-reverse ` ==> 横向排列弹性项目，方向与内容方向相反（**main start 到 main end 的方向与内容方向相反**）

   - ` flex-direction: column ` ==> 纵向排列弹性项目，方向与内容方向相同（**main start 到 main end 的方向与内容方向相同**）

   - ` flex-direction: column-reverse ` ==> 纵向排列弹性项目，方向与内容方向相反（**main start 到 main end 的方向与内容方向相反**）

### flex-wrap
指定 flex 元素是单行显示还是多行显示，可以改变 cross start 到 cross end 的方向

值：
   -   ` flex-wrap: nowrap ` ==> 不允许弹性项目换行

   -   ` flex-wrap: wrap ` ==> 允许弹性项目换行

   -   ` flex-wrap: wrap-server ` ==> 允许弹性项目换行，cross start 到cross end 的方向颠倒

### flex-flow
这是一个集合属性
flex-flow === flex-direction + flex-wrap

### justify-content
**主轴方向**上伸缩项目的对齐方式

值：
   - ` justify-content: space-between ` ==> 在主轴方向上均匀分配弹性项目，相邻弹性项目距离相同。主轴方向上的第一个弹性项目和行首对齐，主轴方向上的最后一个弹性项目和行尾对齐

   - ` justify-content: space-around ` ==> 在主轴方向上均匀分配弹性项目，相邻弹性项目距离相同。主轴方向上第一个弹性项目到行首的距离和主轴方向上最后一个弹性项目到行尾的距离等于相邻元素距离的一半

   - ` justify-content: flex-start ` ==> 弹性项目与主轴起点对齐

   - ` justify-content: flex-end ` ==> 弹性项目与主轴终点对齐

   - ` justify-content: center ` ==> 弹性项目在主轴起点到主轴终点距离的中点

### align-items
**侧轴方向**上伸缩项目的对齐方式

值：
   - ` align-items: flex-start ` ==> 弹性项目项与侧轴起点对齐

   - ` align-items: flex-end ` ==> 弹性项目与侧轴终点对齐

   - ` align-items: center ` ==> 弹性项目在侧轴起点到侧轴终点距离的中点


### align-content
多行或者多列的对齐方式

值：
   - ` align-content: space-between ` ==> 在侧轴方向上均匀分配弹性项目，相邻弹性项目距离相同。主轴方向上的第一个弹性项目和行首对齐，主轴方向上的最后一个弹性项目和行尾对齐


   - ` align-content: space-around ` ==> 在侧轴方向上均匀分配弹性项目，相邻弹性项目距离相同。侧轴方向上第一个弹性项目到行首的距离和侧轴方向上最后一个弹性项目到行尾的距离等于相邻元素距离的一半

   - ` align-content: flex-start ` ==> 弹性项目项与侧轴起点对齐

   - ` align-content: flex-end ` ==> 弹性项目与侧轴终点对齐

   - ` align-content: center ` ==> 弹性项目在侧轴起点到侧轴终点距离的中点
![align-content 属性全解](http://upload-images.jianshu.io/upload_images/9617841-797dc91fac60e84f.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 弹性项目属性

### flex-grow
主轴方向上增长比例。指空间过多时，多余空间分配比例
值： <number> ，负值无效

### flex-shirk
主轴方向上收缩比例。指空间不足时，溢出空间在每个伸缩项目的收缩比例
值： <number> ，负值无效

### flex-basis
主轴方向上伸缩项目的初始大小
值： 
   - <number> ，负值无效
   - ` content ` ==> 基于 flex 的伸缩项目的内容自动调整大小

### flex
flex 是一个复合属性
flex === flex-grow + flex-shirk + flex-basis

### order
主轴方向上弹性项目布局时的顺序
值：<number> ，负值有效

### align-self
自身的对齐方式
值：
   - ` align-self: flex-start ` ==> 弹性项目项与侧轴起点对齐

   - ` align-self: flex-end ` ==> 弹性项目与侧轴终点对齐

   - ` align-self: center ` ==> 弹性项目在侧轴起点到侧轴终点距离的中点

# 最佳实践 & 套路
1. 垂直水平居中
```
display: flex;
justify-content: center;
align-items: center;
```

# 相关知识点
这里有两个小游戏，可以练习 flex 属性
[网站一](http://flexboxfroggy.com/#zh-cn)
[网站二](http://www.flexboxdefense.com/) 