---
title: CSS 最佳实践 + 套路（五） -- icon
date: 2018-01-27 07:58:06
tags: CSS
---
# 概述
icon 有很多做法，主要的做法有
- img 标签
- background-image 属性
- CSS Sprites 
- font
- svg
- 纯 CSS 实现

在这些方法中，目前最值得推荐的就是 **svg 方法**，其他的大家可以了解一下。

# img 标签
```
<img src="./img/xxx.png" alt="icon">
```
此种方法可以自适应宽高，只需要给 img 一个 width 属性或者一个 height 属性即可

# background-image 属性
```
CSS：
    div{
        background-image: url(./image/xxx.png);
    }

HTML:
    <div></div>
```

# CSS Sprites 
可以使用[工具自动生成 Sprites](http://css.spritegen.com/) ，之后进行使用，此种方法便是工具的使用

# font
font 方法主要是使用[阿里](http://www.iconfont.cn/)的 Unicode 和 Font class 方法
![Font 方法](http://upload-images.jianshu.io/upload_images/9617841-ca3bfe98993983c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# svg
svg 方法是现在最值得推荐的方法
**SVG**是可缩放矢量图形，是W3C XML的分支语言之一，用于标记可缩放的矢量图形。它支持 CSS 属性

首先去[阿里的 iconfont 网站](http://www.iconfont.cn/)
1. 选取相应的图标并加入购物车
2. 将购物车中的图标添加至项目
3. 生成 Symbol 代码
4. 拷贝生成的 Symbol 代码，放在 HTML 中
5. 引入通用css代码

    ```
    <style type="text/css">
        .icon {
           width: 1em; height: 1em;
           vertical-align: -0.15em;
           fill: currentColor;
           overflow: hidden;
        }
    </style>
    ```

6. 挑选相应图标并获取类名，应用于页面

    ```
    <svg class="icon" aria-hidden="true">
        <use xlink:href="#icon-xxx"></use>
    </svg>
    ``` 
![SVG](http://upload-images.jianshu.io/upload_images/9617841-1b6964ececccf5f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![阿里 iconfont 使用](http://upload-images.jianshu.io/upload_images/9617841-ccdc31dd20104add.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`<svg>` 属性：
```
width:          // icon 宽度
height:          // icon 高度
fill:          // icon 颜色
stroke:    // 描边
vertical-align: top;   //可以用于调整上下间距。
```
# 纯 CSS 实现
纯 CSS 实现就是使用 CSS 去画一些图标，[推荐一个网站](http://cssicon.space/#/)