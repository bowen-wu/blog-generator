---
title: 总结篇（二） -- 基础知识之 CSS
date: 2018-02-05 22:54:05
tags: 总结
---
1. 选择器
    - 标签选择器
    - 类选择器
    - ID 选择器
    - 后代选择器
    - 子选择器
    - 通用选择器
    - 伪类选择符
    - 分组选择符

2. 文档流
    内联元素 ==> 从左向右依次流动
    块级元素 ==> 从上到下依次流动，每个块级元素独占一行
3. 盒模型
    box-sizing: content-box; ==> 标准盒模型，width = contentWidth
    box-sizing: border-box; ==> width = contentWidth + padding + border
4. 伪类
    伪类有动态伪类、结构伪类、否定伪类等等
    - 动态伪类：
    ` :link ` ==> 未访问前的样式效果
    ` :hover ` ==> 鼠标悬停时的样式效果
    ` :active ` ==> 鼠标点击时的样式效果
    ` :visited ` ==> 访问后的样式效果
    ` :focus ` ==> 元素成为焦点时的样式效果
    - 结构伪类
    ` :first-child ` ==> 第一个子元素
    ` :last-child ` ==> 最后一个子元素
    ` :nth-child(n) ` ==> 第 n 个子元素
    - 否定伪类
    ` :not ` ==> 不符合参数选择器 X 描述的元素
5. 伪元素
    - ` ::before ` ==> 创建伪元素，此伪元素是元素的第一个子元素
    - ` ::after ` ==> 创建伪元素，此伪元素是元素的最后一个子元素
6. 堆叠上下文（BFC）
    BFC 就是块级格式化上下文。形成 BFC 条件：
    - 浮动
    - 绝对定位元素（` position: absolute; `）
    - 非块盒的块容器（` display: inline-block; | display: table-cells `）
    - overflow 不为 visible 的块盒
    - ` display: flow-root `

    功能：
    1. 将内部浮动元素包裹起来
    2. 两个相邻的 BFC 之间划清界限
7. 媒体查询
    ` <link> ` 标签中的媒体查询
    ```
    <link rel="stylesheet" metia="(max-width: 800px)" href="xxx.css">
    ``` 

    样式表中的 CSS 媒体查询
    ```
    <style>
        @media (max-width: 800px) and (min-width: 600px) {
            // 选择器 + 样式
        }
    </style>
    ```
8. 动态 REM
    rem 是相对单位长度，它是根元素 ` <html> ` 的 ` font-size ` 的大小，网页默认的 ` font-size: 16px `，运用 JS 探取屏幕宽度，之后定义根元素的 ` 
font-size ` 与屏幕宽度相关，之后一切单位都以屏幕宽度为基准
    ```
    <script>
        let pageWidth = window.innerWidth
        document.write( '<style>html{ font-size:' + pageWidth/10 + 'px;}</style>' )
    </script>
    ```
9. box-shadow
    - 垂直偏移
    - 水平偏移
    - 模糊半径
    - 模糊尺寸
    - 颜色
10. transform
    - ` translate ` ==> 移动
    - ` rotate ` ==> 旋转
    - ` skew ` ==> 倾斜
    - ` scale ` ==> 缩放
11. 帧动画
    - ` animation-name ` ==> 动画名称
    - ` animation-duration ` ==> 持续时间
    - ` animation-delay ` ==> 延迟
    - ` animation-timing-function ` ==> 动画类型
    - ` @keyframe ` ==> 关键帧