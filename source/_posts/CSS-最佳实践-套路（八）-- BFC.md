---
title: CSS 最佳实践 + 套路（八） -- BFC
date: 2018-01-28 09:38:32
tags: CSS
---
# 概述
BFC（Block Formatting Context）块级格式化上下文。我并不知道 BFC 是什么，但是只要写出样式，我就知道这是不是 BFC。BFC 没有明确的定义，只有特征或功能。

# 形成 BFC 条件
1. 浮动（` float: left | right `）

2. 绝对定位元素（` position: absolute `）

3. 非块盒的块容器（` display: inline-block | table-cells | table-captions `）

4. overflow 不为 visible 的块盒（` visible: auto | hidden | scroll `）

5. ` display: flow-root ` ==> 专门用于使得当前元素触发 BFC

# 功能
1. 将内部浮动元素包裹起来
使用 BFC 将其内部浮动元素包裹起来
![BFC 将内部浮动元素包裹起来](http://upload-images.jianshu.io/upload_images/9617841-747425ae0fda01ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[代码地址](http://js.jirengu.com/kazusiciwo/1/edit)
2. 两个相邻的 BFC 划清界限
float + div 左右自适应布局
![BFC 两个相邻 BFC 划清界限](http://upload-images.jianshu.io/upload_images/9617841-fd7b91203b1d28b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[代码地址](http://js.jirengu.com/suyowimizi/1/edit)

# BFC 与 文档流
**BFC** 是本身特性，它会影响元素的宽高
**文档流**指内联元素从左到右排列，块级元素从上到下排列。它会影响元素的排列顺序
二者没有关系
BFC 里面有文档流，BFC 本身也可以是文档流

# 最佳实践 & 套路
不要去使用 BFC
1. 清除浮动使用 ` clearfix `
    ```
    clearfix::after{
        content: '';
        display: block;
        clear: both;
    }
    ```
2. 划清界限使用 ` display: flex `
使用 ` display: flex + flex: 1 `
![display: flex + flex: 1](http://upload-images.jianshu.io/upload_images/9617841-10a88b81f6380059.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 相关知识点
BFC 内部相邻块级元素竖直 margin 合并
