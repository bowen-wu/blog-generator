---
title: CSS 最佳实践 + 套路（九） -- Bootstrap
date: 2018-01-28 09:42:23
tags: CSS
---
# 概述
Bootstrap 是一个用于前端开发的开源工具包，Bootstrap 可以适配所有设备，主要就是采用了媒体查询功能，当我们知道它是怎么实现的了，那么它也就不那么神秘了。当我们在使用 Bootstrap 时，主要需要掌握的就是**栅格布局**，剩下的就是复制粘贴了。
[英文官网](https://getbootstrap.com/)
[中文官网](http://www.bootcss.com/)

# 准备工作
开始使用 Bootstrap 之前我们需要引入 Bootstrap
```
<link rel="stylesheet" href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" >
<script src="https://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js">
```
之后我们还需要引入一个 meta 标签
```
<meta name="viewport" content="width=device-width, initial-scale=1, maximun-scale=1, minimum-scale=1, user-scalable=no"
```
# 栅格系统
栅格系统主要就是将屏幕分成12等份。栅格系统主要创建在布局容器中，布局容器中存放行（row）和列（column），每行只能有12列，超出元素将作为一个整体存放在下一行中

### 布局容器
布局容器主要有两个类
` .container ` ==> 固定宽度并支持响应式布局的容器，主要用于 PC 端
` .container-fluid ` ==> 100% 宽度，占据全部视口 viewport 的容器，主要用于移动端
**注意：**我们所要做的就是将我们的所有内容都使用布局容器盛放

### 行
行表示在水平方向上创建一行，它的类只有一个` .row ` 

### 栅格参数
栅格参数主要就是表示栅格系统是如何在页面中显示的
![栅格参数](http://upload-images.jianshu.io/upload_images/9617841-8ed0166a152ca66a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在使用的时候我们需要将我们的内容放在每个行中，如果我们需要加一些 CSS 属性，此时需要做的便是加一层 div ，用来添加自己的样式
**示例：**
```
<div class="container">
    <div class="row">
        <div style="padding: 10px;">  // 这个就是我们添加的 div，用来添加我们自己的样式
            我们的内容
        </div>
    </div>
</div>
```

# 相关知识点
- 生产环境：程序所在的机器 | 操作系统面向真实的用户
- 开发环境：只给自己 | 内部人员使用的程序，例如 ` http-server `

以上便是 Bootstrap 需要了解的知识，剩下的便是去官方网站粘贴复制，个人推荐：不要去使用 Bootstrap，因为 CSS 是一个牵一发而动全身的技术，如果你选择了 Bootstrap，那么你的想象力将会受到限制