---
title: CSS 最佳实践 + 套路（一） -- 概述
date: 2018-01-26 10:51:19
tags: CSS
---
# 资源
1.  Google: 关键词 MDN
2.  [CSS Tricks](https://css-tricks.com/ "null")
3.  [Google: 阮一峰 css](https://www.google.com/search?q=%E9%98%AE%E4%B8%80%E5%B3%B0+css "null")
4.  [张鑫旭的 240 多篇 CSS 博客](http://www.zhangxinxu.com/wordpress/category/css/page/25/ "null")
5.  [Codrops 炫酷 CSS 效果](https://tympanus.net/codrops/category/playground/ "null")
6.  [CSS揭秘](http://www.ituring.com.cn/book/1695 "null")
7.  [CSS 2.1 中文 spec](http://cndevdocs.com/ "null")
8.  [Magic of CSS](http://adamschwartz.co/magic-of-css/ "null") 免费在线书
# 引入CSS
1. 内联样式 ==> style属性 ==> `style= 'color: red; width: 200px; height:200px;'`
2. 内嵌样式 ==> `style` 标签 ==> 
```
<style>
  body{
    background: gray;
    font-size: 12px;
  }
</style>
```
3. 外联样式 ==> `<link>` 标签 ==> 
```
<link rel= 'stylesheet' href= ''>
```
4. `@import url(./b/css)`


# 最佳实践 & 套路

### reset CSS
    ```
    *{ margin: 0; padding: 0; }
    h1, h2, h3, h4, h5, h6, p{ margin: 0; padding: 0; }
    ```

### 清除浮动
添加到浮动元素的父元素上。
```
.clearfix::after{
    content: '';
    display: block;
    clear: both;
}
```

# 相关知识点
元素分类：
- 块级元素：`block` ==> `display: block;`
- 内联元素：`inline` ==> `display: inline;`
- 内联块级元素：`inline-block` ==> `display: inline-block;`