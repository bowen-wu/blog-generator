---
title: CSS 最佳实践 + 套路（四） -- 移动端适配
date: 2018-01-26 10:57:26
tags: CSS
---
[脚本实现自动创建移动端适配的相关文件](https://github.com/bowen-wu/node-create-file)
# 概述
手机上显示网页是将整个网页缩小，模拟页面宽度 980px ，可以使用 ` document.documentElement.clientWidth ` 来获取到页面宽度为 980px。在移动端加上 ` <meta> ` 标签才是自适应，否则就是 980px ，即网页整体缩小。

# meta 标签
```
<meta name=' viewport ' content=' width=device-width user-scalable=no initial-scale=1.0 maximum=1.0 minimum-scale=1.0 '>
```
**说明：**
- ` width=device-width ` ==> 网页宽度默认等于屏幕宽度
- ` user-scalable=no ` ==> 用户是否可以缩放
- ` initial-scale=1.0 ` ==> 原始缩放比例为 1.0 ，即网页初始大小占屏幕面积的 100%
- ` maximum=1.0 ` ==> 最大缩放比例
- ` minimum-scale=1.0 ` ==> 最小缩放比例

` <meta> ` 标签可以防止手机模拟 980px，防止页面在用户双击的时候放大和防止用户横屏


# Media Query （媒体查询）模块
由一个可选的媒体类型和零个或多个使用媒体功能的限制了样式表范围的表达式组成，即**对媒体进行某些条件的查询**

1. 目前来说只用两种模式 ：
    - 移动端 
    - PC 端 

2. 两种技术：
    - **mobiles-first** ==> 先做移动端页面后做 PC 端页面（推荐）
    - desktop-first ==> 先做 PC 端页面后做移动端页面

3. 两种方案：
    - 移动端和 PC 端分别跳转不同网站（京东、淘宝）
    - 渲染不同的 HTML+CSS（知乎）

### <link> 元素中的 CSS 媒体查询
满足查询条件，则生效，否则不生效。但是无论生效与否**都会下载**
```
<link rel="stylesheet" media="(max-width: 800px)" href="example.css" />
```

### 样式表中的CSS媒体查询
满足查询条件，则生效，否则不生效。
```
<style>
@media (max-width: 600px) {
  .facet_sidebar {
    display: none;
  }
}
</style>
```

### 操作符
- and 操作符 ==> 把多个媒体属性组合成一条媒体查询，对成链式的特征进行请求，只有当每个属性都为真时，结果才为真
```
(min-width: 700px) and (max-width: 960px) { ... }
```

- 逗号分隔列表 ==> 使用逗号分隔效果等同于**or逻辑操作符**。当使用逗号分隔的媒体查询时，如果**任何一个媒体查询返回真**，**样式**就是**有效**的
```
@media (min-width: 700px), handheld and (orientation: landscape) { ... }
```

- not 操作符 ==> 应用于整个媒体查询，在媒体查询为假时返回真

- only 操作符 ==> 防止老旧的浏览器不支持带媒体属性的查询而应用到给定的样式
```
<link rel="stylesheet" media="only screen and (color)" href="example.css" />
```

**说明：**若使用了not或only操作符，必须明确指定一个媒体类型。

### 媒体特征
- device-width ==> 设备宽度
- width ==> 宽度。描述了输出设备渲染区域的宽度。接受 **mix/min** 前缀

### 响应式
响应式即不同的屏幕得到不同的样式

# 长度单位
- px ==> 绝对长度单位，像素
- em ==> 相对长度单位，这个单位表示**元素的 `font-size` 的计算值**。
- rem ==> 相对长度单位，这个单位代表**根元素（<html>）**的 `font-size` 大小
**说明：网页默认的 ` font-size: 16px `**
- vh ==> viewport hight，相对长度单位，视口高度的 1/100。
- vw ==> viewport width，相对长度单位，视口宽度的 1/100。
**说明：vh 和 vw 的兼容性比较差，使用较少**

# 动态 REM
由于手机上分辨率太多，不是做响应式，只能退而求其次
- **百分比布局**：高度问题，宽度不知道具体大小，仅仅知道百分比。造成了宽度不确定导致高度和宽度不能做任何配合
- **动态REM**：一切单位以宽度为基准（vw 啊，但是 vw 兼容性较差），退而求其次：**rem**

**rem ==> html font-size   =（JS实现）=> page width（页面宽度）**
```
<head>
    <script>
        var pageWidth = window.innerWidth
        document.write( '<style>html{font-size:' + pageWidth/10 + 'px;}</style>' )
    </script>
    <style>
        body{
            font-size: 16px;
        }
    </style>
</head>
```
将 rem 变为 pageWidth/100 即可以表示 vw（视口宽度的 1/100），**Chrome 浏览器默认最小字号 12px**，此时的 ` font-size ` 被 Chrome 浏览器强制为 **12px**。所以将 rem 变为 pageWidth/10 。

# 最佳实践 & 套路
### SASS 实现将 px 自动变为 rem
[LUNIX 安装SASS](https://www.jianshu.com/p/f1fccdb53f8a)
SASS 文件内容
```
@function px( $px ){
  @return $px/$designWidth*10 + rem; // 此处的 10 是 html{font-size:' + pageWidth/10 + 'px;} 中的10 
}
$designWidth : 640; // 640 是设计稿的宽度，需要根据设计稿的宽度填写
```


# 相关知识点
1. 移动端中没有 hover 事件，但是有 **[touch 事件](https://www.jianshu.com/p/f0df9575a101)**
2. 移动端中**没有 resize 事件**，并且没有滚动条（隐藏滚动条）
2. 使用相对大小的字体 rem
3. 流动布局，各个区块都是浮动的
4. 选择加载 CSS ，Media Query 模块（CSS3）自动探测屏幕宽度，然后加载相应的 CSS 文件
5. CSS 的 @media 规则
6. 图片自适应 max-width: 100%;
7. 很小很小的地方使用 px ==> 1px
8. rem 单位和其他单位混用，例如：` font-size: 16px `