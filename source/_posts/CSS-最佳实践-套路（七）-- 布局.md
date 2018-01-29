---
title: CSS 最佳实践 + 套路（七） -- 布局
date: 2018-01-27 08:02:20
tags: CSS
---
# 概述
在我们平时布局的时候，一般都是选择 Float 布局和 Flex 布局。如果项目需要支持 IE8 ，那么推荐选择 Float 布局，如果不需要支持 IE8 ，那么尽量选择 Flex 布局，当我们不需要支持 IE8 的时候，那么我们就能使用很多高级的语法，例如：calc()

# 原则
1. 不要写死 ` width ` 和 ` height `
2. 尽量选择 flex、calc
3. 如果是 IE ，那么上述两点全部推翻，能写死就写死

# Float 布局
1. 子元素：` float: left | right `
2. 父元素：添加 ` clearfix ` 类
    ```
    clearfix::after{
        content: '';
        display: block;
        clear: both;
    }
    ```

# Flex 布局
在 [CSS 最佳实践 + 套路（六） -- flex](https://www.jianshu.com/p/f3a85c014952) 一文中已经详细介绍了关于 FLex 的相关属性。Flex 布局主要是

1. 父元素： ` display: flex; ` + 弹性容器相关属性
2. 子元素：弹性项目相关属性

# 最佳实践 & 套路
实现这种布局
![布局](http://upload-images.jianshu.io/upload_images/9617841-c9e504af91cce8b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
每个模块都是一样的，四个模块占据一行，中间有 margin ，四个模块占据的宽度是一定的。
1. float + margin 负值法
HTML：
    ```
    <div class="container">
        <div class="inner">
            <ol class="clearfix">
                li*10
            </ol>
        </div>
    </div>
    ```
    CSS:
    ```
    .container{
        width: 960px;
        margin: 0 auto;
    }
    .clearfix{
        content: '';
        display: block;
        clear: both;
    }
    .inner{
        margin: 0 -间距/2px；
    }
    li{
        width: 
        float: left;
    }
    ```

2. flex + margin负值法
HTML：
    ```
    <div class="container">
        <div class="inner">
            <ol>
                li*10
            </ol>
        </div>
    </div>
    ```
    CSS:
    ```
    .container{
        width: 960px;
        margin: 0 auto;
    }
    ol{
        display: flex;
        justify-content: space-between;
        margin: 0 -间距/2px；
    }
    ```

3. flex + calc
HTML：
    ```
    <div class="container">
        <div class="inner">
            <ol>
                li*10
            </ol>
        </div>
    </div>
    ```
    CSS:
    ```
    .container{
        width: 960px;
        margin: 0 auto;
    }
    ol{
        display: flex;
        justify-content: space-between;
    }
    li{
        calc(25% - 8px)     //20%*父元素宽度 - 8px
    }
    ```