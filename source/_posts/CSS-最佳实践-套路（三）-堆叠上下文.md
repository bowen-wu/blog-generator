---
title: CSS 最佳实践 + 套路（三） -- 堆叠上下文
date: 2018-01-26 10:54:17
tags: CSS
---
# 条件
文档中的层叠上下文由满足以下任意一个条件的元素形成：
- 根元素（HTML）
- **z-index 不为 auto 的 absolute/relative**
- 一个 z-index 值不为 auto 的 flex 项目，即父元素 display: flex/inline-flex
- opacity 属性值小于 1 的元素
- transform 属性不为 none 的元素
- mix-blend-mode 属性值不为 normal 的元素
- filter 不为 none 的元素
- perspective 值不为 none 的元素
- isolation 属性被设置为 isolate 的元素
- position: fixed
- 在 will-change 中指定了任意 CSS 属性，即使没有直接指定这些属性的值
- -webkit-overflow-scrolling 属性被设置为 touch 的元素

#堆叠层级：
```
负z-index(父元素没有position: relative/absolute) < position: static（background-color < border < 负z-index(父元素position: relative/absolute)  < div/块级元素 < 浮动元素 < 浮动元素内的文字/内联元素 < 浮动元素外面的文字/内联元素） < position: relative/absolute < 正z-index
```
- 相同的属性按先后顺序排列
- 具有相同 ` position ` 属性的 + ` z-index ` 只能覆盖相同 ` position ` 属性（` relative === absolute `）的元素


# 作用 + 影响
堆叠上下文主要是影响 ` z-index `
` z-index: 2 ` 永远在 ` z-index: 0 ` 的上方么？ 
![堆叠上下文影响 z-index 01](http://upload-images.jianshu.io/upload_images/9617841-d931d5aaeb0f0ab7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![堆叠上下文影响 z-index 02](http://upload-images.jianshu.io/upload_images/9617841-461257c3d745833f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![堆叠上下文影响 z-index 03](http://upload-images.jianshu.io/upload_images/9617841-5b575d02cd7bb458.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![堆叠上下文影响 z-index 04](http://upload-images.jianshu.io/upload_images/9617841-c35c60561e808a33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![堆叠上下文影响 z-index 05](http://upload-images.jianshu.io/upload_images/9617841-0ee2aa3c1c265b62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![具体使用示例](http://upload-images.jianshu.io/upload_images/9617841-868517d973ef2db3.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 最佳实践 & 套路
` z-index ` 需要配合 ` position: relative | absolute ` 使用