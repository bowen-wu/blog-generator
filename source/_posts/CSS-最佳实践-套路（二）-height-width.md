---
title: CSS 最佳实践 + 套路（二） -- height width
date: 2018-01-26 10:53:08
tags: CSS
---
# 文档流
文档内元素的流动方向

- 内联元素 ==> 从左向右依次流动，若 `width` 不足，则换行
- 块级元素 ==> 从上到下依次流动，每个块级元素独占一行

# 脱离文档流
将会影响其祖先元素（块级元素）的 height 

脱离文档流的三种方式：
- `position: fixed` 相对于窗口定位
- `position: absolute` 相对于定位包含框定位（`position: absolute; top: 100%;`）
- `float: ` 浮动，可以利用 `.clearfix` 类清除浮动

# height 问题
- `div` （块级元素）==> 由其内部文档流元素的 `height` 总和**决定**。
- `span` （内联元素） ==> `span` 的 `height` 决定于具体的字体（**建议行高** + 设计字体的设计师决定）。

### 建议行高
字体都有一个**建议行高**，每种字体的**建议行高**是不同的。以下面的 `span` 为例：
```
<span style= 'font-family: 字体A; font-size: 20px; line-height: 40px;'>字体hug</span>
<span style= 'font-family: 字体B; font-size: 20px; line-height: 40px;'>字体hug</span>
```
- 两个 `span` 的字体不同，所以两个 `span` 的 `height` 就不相同。
-  `font-size: 20px;` 指的是英文字母 `h` 的上部 **距** 英文字母 `g` 的下部的距离为 `20px` ，中文汉字会比 `20px` 偏小一些。

# 最佳实践
1. 将 `line-height` 设置的比 `font-size` 大一些，那么行内元素的 `height` 将会等于 `line-height` 的值

2. 内联元素设置 `width` 和 `height` 时，不要使用
    ```
    display: block;
    height: 
    line-height: 
    text-align: center;
    ```
    **通过添加 `padding` 从而达到想要的 `width` 和 `height` （添加 `line-height` 明确 `height`），并且宽度（max-width）自适应**
    ```
    padding: 
    line-height: 
    ```

# 套路
### 设置一个 `height` 为 `40px` （近似）的 `div` ,其内部包含内敛元素 `span` ：
```
<div style= 'line-height: 24px; border: 1px solid green; padding: 6px 0;'>
    <span style= 'font-size: 14px; border: 1px solid red;'>hug</span>
</div>
```

### 两个 ` span ` 两端对齐方法
` text-align: justify ` ==> 定义行内内容（例如文字）如何相对它的块父    元素对齐。`text-align` 并不控制块元素自己的对齐，只控制它的行内内容    的对齐。**文字向两侧对齐（必须是多行文本）**。
```
    span{ 
      display: block;
      width:            // 设置宽度，从而让两端对齐
      line-height:    //设置行高和高度，固定下来
      height: 
      taxt-align: justify;  //设置两端对齐 
      overflow: hidden;  
    }
    span::after{   // 设置伪类，从而有第二行。
      content: '';
      display: inline-block;
      width: 100%;
    }
```

**注意：**可以将 ` span ` 设置为 ` display: inline-block ` ，之后在第一个 ` span ` 后面加上 ` <br> ` 

![两端对齐](http://upload-images.jianshu.io/upload_images/9617841-6bb605df69f4cd63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 文字省略溢出
##### 单行文本省略溢出
```
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis
```
##### 多行文本省略溢出（css multi line text ellipsis）
```
display: -webkit-box;
-webkit-line-clamp: 3; //控制行数
-webkit-box-orient: vertical;
overflow: hidden;
```

### 实现一个 1:1 的 div
```
border: 1px solid red;
padding-top: 100%;
```

### ` margin ` 合并
一个 ` div ` 标签中有一个子标签 ` div ` ，如果父标签有以下属性，则子标签中的 ` margin `（上下） 属性不会合并。
- border：
- padding: 
- overflow: hidden;( 不推荐 )


# 相关知识点

1. 文字和单词、单词和单词都是以**基线**对齐。

2. 内联元素的 ` padding ` 、 ` margin ` 和 ` border ` 不会影响 ` height ` ，但是会影响 ` 
width `