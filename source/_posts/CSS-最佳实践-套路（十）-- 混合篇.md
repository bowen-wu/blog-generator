---
title: CSS 最佳实践 + 套路（十） -- 混合篇
date: 2018-01-28 09:45:02
tags: CSS
---
# 概述
本文章主要就是将之前几篇文章中没有提到的，但是个人认为比较重要的 CSS 的最佳实践和套路
## 居中

#### 水平居中：
- 内联元素：父元素加 ` text-align:center; ` 
- 块级元素（**固定宽度**）：` margin: 0 auto; `
- 元素不是上述两种情况：
        1. 将元素变成内联元素　` display: inline-block; ` + ` text-align:center; ` + ` vertical-align: top `
        2. 固定宽度（不推荐）

#### 垂直居中
- 内联元素：` line-height: height; `
- 块级元素：` line-height ` + ` padding ` 构造高度，达到垂直居中（凑高度）

#### 定位居中法
- 宽度固定
    ```
    position: absolute;
    left: 50%;
    top: 50%;
    margin-top: -width/2;
    margin-left: -heigth/2
    ```

- 宽度不固定
    ```
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%)   // CSS3 属性
    ```

#### 弹出层或者 loading 定位居中
```
position: absolute; // 此种方法需要配合定宽定高
top: 0;
bottom: 0;
left: 0;
right: 0;
margin: auto;
```

#### Flex 布局居中
```
display: flex;
justify-content: center;
align-items: center;
```

## 纯 CSS 实现三角形
- first
    ```
    div.first{
      border:10px solid transparent;
      width:0;
      border-top-color: red;
    }
    ```

    ![三角形-first.png](http://upload-images.jianshu.io/upload_images/9617841-077b0d7ad37b7982.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- second
    ```
    div.second{
      border:40px solid transparent;
      width:0;
      border-top-width:0;
      border-left-color: red;
    }
    ```

    ![三角形-second.png](http://upload-images.jianshu.io/upload_images/9617841-cd63ba6dd15b2ba9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- third
    ```
    div.third{
      border:20px solid transparent;
      width:0;
      border-top-color: red;
      border-left-color: red;
    }
    ```

    ![三角形-third.png](http://upload-images.jianshu.io/upload_images/9617841-fc99fb26421f7ca5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里推荐一个[使用纯 CSS 实现各种图形](https://css-tricks.com/examples/ShapesOfCSS/)的网站

## 两个元素重叠一部分
![效果图](http://upload-images.jianshu.io/upload_images/9617841-64958b12dd693d58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**方法：**
- `margin` 负值法： `margin-top: - `

- CSS3 transform属性： `transform: translate(- )` [ 原始位置保留不变，下方元素和 `transform` 元素有**间隔** ]

- 相对定位：`position: relative; top(- )` [ 原始位置保留不变，下方元素和相对定位元素有**间隔** ]

## 内联元素消除空隙
当元素为内联元素 | 内联块级元素时，该元素下方会有小空隙，消除空隙 
```
display: inline | inline-block;
vertical-align: top;
```

## 如果想要整个页面的遮罩层，需要使用 
``` 
position: fixed;
width: 100%;
height: 100%; 
```
不能使用 ` position: absolute; ` ，其高度不是页面的高度，是窗口的高度

## 伪类
CSS伪类是添加到选择器的关键字，指定要选择的元素的特殊状态。

- 动态伪类
    ```
    :link ==> 未访问前的样式效果
    :hover ==> 鼠标悬停时的样式效果
    :active ==> 鼠标点击时的样式效果
    :visited ==> 访问后的样式效果
    :focus ==> 元素成为焦点时的样式效果
    ```

- 结构伪类（CSS选择符）
    ```
    :first-child ==> 第一个子元素
    :last-child ==> 最后一个子元素
    :nth-child(n) ==> 第 n 个子元素
    :nth-child(2n) === :nth-child(even) ==> 偶数元素
    :nth-child(2n+1) === :nth-child(odd) ==> 奇数元素
    ```

- 否定伪类（CSS选择符）
    ```
    :not() ==> 不符合参数选择器 X 描述的元素。
    ```

## 伪元素
伪元素添加到选择器，但不是描述特殊状态，它们可以为元素（**非空元素**）的某些部分设置样式。

- `::before` 为当前元素创建一个**伪元素**，做为已选中元素的**第一个**子元素。常通过 `content` 属性来为该元素添加修饰性的内容。 这个**虚拟元素**默认是行内元素。

- `::after` 为当前元素创建一个**伪元素**，做为已选中元素的**最后一个**子元素。常通过 `content` 属性来为该元素添加修饰性的内容。这个**虚拟元素**默认是行内元素。

```
div::before{
    content: '';  // 必须有，才能显示出内容。
}
div::after{
    content: '';   // 必须有，才能显示出内容。
}
```

## CSS 属性
1. `work-break: break-all` 打断所有的单词，从而使得词语（单词）换行

2. 圆：`border-radius: 50%;` 

3. `position` 属性：用于指定一个元素在文档中的定位方式。值：

    ```
    static：该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。
    relative  
    absolute  
    fixed
    sticky（粘性定位[ 粘性定位是相对定位和固定定位的混合 ]）：盒位置根据正常流计算(这称为正常流动中的位置)，然后相对于该元素在流中的 flow root（BFC）和 containing block（最近的块级祖先元素）定位
    ```

4. `box-sizing` 属性：用于更改用于计算元素宽度和高度的默认的 CSS 盒子模型。
**值：**
    - `content-box`：默认值，标准盒子模型。`width` 与 `height`只包括内容的宽和高， 不包括边框（border），内边距（padding），外边距（margin）。
    **维度计算：**
    `width` = 内容的  width
    `height` = 内容的 height。
    - `border-box`： `width` 和 `height`属性包括内容，内边距和边框，但不包括外边距。
    **维度计算：**
    `width` = border + padding + 内容的  width
    `height` = border + padding + 内容的 height

5. `display: inline` 元素
    - 设置 `width` 和 `height` 是无效的
    - 设置 `margin-top` 和 `margin-bottom` 是无效的（解决方案：设置为 `display: inline-block; vertical-align: top;` ，如果此时该元素要居中，给其设置一个**行内父元素**，之后此**行内父元素** `text-align: center;`）
    - 设置 `margin-left` 和 `margin-right` 是**有效**的

## 小技巧 & 注意点

1. `img` 的 `height` 和 `width` 写在 `img` 标签中，作为标签属性，这样可以避免重排的发生从而浪费了 CPU 性能（页面性能优化）。
    ```
    <img src= '' width= 200 height= 300 alt= ''>
    ```

2. `hr` 的**最佳实践**
    ```
    .hr{
        border: none;
        border-top: 1px solid color;
    }
    ```

3. `span` 元素内可以包裹 `span` ，不能包裹块级元素 `div` ，可以为 `span` 设置 `display: block` 从而达到效果

4. `li` 里面有 `a` 元素时，如果 `a` 元素有 `padding` ，则 `li` 不能完全包裹 `a` ，此时需要给 `a` 添加 `display: block;` 属性即可。

5. CSS中 `width` 的默认值是 **`auto`**

6. 如果要继承父元素的颜色，要声明 ` color: inherit; `

7. ` animation ` 动画的时候要注意使用 ` opacity `

8. vh（viewport height）：视图高度单位，100vh：表示整个视图高度

9. 属性 `text-transform: uppercase` 将小写字母转化为大写字母

10. position: absolute; 时，使用 **top: 100%;**，而不使用 bottom: 0;

11. 注意使用 `white-space: nowrap` ，强制文本不换行，从而避免使用 `width` （在创建二级菜单时尤其注意）

12. 有时父元素不能完全包裹住带有 `padding` 的子元素，此时可以尝试给子元素添加 `display: inline-block; vertical-align: top;` 或者 `display: block;`