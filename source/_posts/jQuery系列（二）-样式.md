---
title: jQuery系列（二） -- 样式
date: 2018-01-12 19:05:51
tags: jQuery
---
# 概述
jQuery 函数包装的对象是类数组对象，对象里面包含了 DOM 对象的信息，并封装了很多操作方法，可以和访问数组一样的方法来访问。

# jQuery 对象 和 DOM 对象
### 联系
可是使用 $ 符号将 DOM 对象封装成 jQuery 对象 ` $( div ) ` 
可是使用 ` div = $div[ 0 ] ` 或者 ` div = $div.get( 0 )  ` 将 jQuery 对象转化成 DOM 对象
###区别
DOM 对象的属性和方法有： .style   .classList       
jQuery 对象的属性和方法有：.css     .addClass    

**说明：**传递给 $(DOM) 函数的参数是 DOM Object ，则 jQuery 函数会把这个 DOM Object 给包装成一个新的 jQuery 对象

# jQuery 选择器
1. id 选择器： $( "#id" )
2. 类选择器：$( ".class" )
3. 元素选择器： $( "element" )
4. 全选择器：$( "*" )
5. 层级选择器
    - 子选择器：$( "parent > child" )

    - 后代选择器：$( "ancestor descendant" )
    
    - 相邻兄弟选择器：$( "prev + next" )
    选择所有紧跟在 ` prev ` 元素后的 ` next ` 元素

    - 一般兄弟选择器：$( "prev ~ siblings" )
匹配 ` prev ` 元素之后的所有兄弟元素，具有相同的父元素，并匹配过滤 ` 
siblings ` 选择器

6. 基本筛选选择器
    - $( ":first" ) ==> 匹配第一个元素
    - $( ":last" ) ==> 匹配最后一个元素
    - $( ":not(seletor)" ) ==> 选择所有元素去除不匹配给定的选择器元素
    - $( ":eq(index)" ) ==> 在匹配的集合中选择 index 的元素（从0开始）
    - $( ":gt(index)" ) ==> 在匹配的集合中选择大于 index 的元素（从0开    始）
    - $( ":lt(index)" ) ==> 在匹配的集合中选择小于 index 的元素（从0开    始）
    - $( ":odd" ) ==> 选择索引值为奇数的元素
    - $( ":even" ) ==> 选择索引值为偶数的元素
    - $( ":header" ) ==> 选择所有标题元素
    - $( ":root" ) ==> 选择该文档的根元素
    - $( ":lang(language)" ) ==> 选择指定语言的所有元素
    - $( ":animated" ) ==> 选择所有正在执行动画的元素
    **说明：**
            1. :eq()、:lt()、:gt()、:even、:odd 用来筛选他们前面的匹配表达式的集合元素，根据之前匹配的元素进一步筛选
            2. :gt() 是一个段落筛选，从指定索引的下一个开始，:gt(1) 实际是从2开始

7. 内容筛选选择器
    - $( ":parent" ) ==> 选择所有含有子元素或者文本的元素

    - $( ":empty" ) ==> 选择所有没有子元素的元素（包括文本节点）

    - $( ":contains(text)" ) ==> 选择所有包含指定文本的元素

    - $( ":has(selector)" ) ==> 选择元素中至少包含指定选择器的元素
    **说明：**
            1. :contain 和 :has 都有查找的意思，但 contains 查找包含**制定文本**的元素，has 查找包含**指定元素**的元素
            2. 如果 :contains 匹配的文本包含在元素的子元素中，同样认为是符合条件的
            3. :parent 和 :empty 是相反的，但是两者所涉及的子元素都包括文本节点

8. 可见性筛选选择器
    - $( ":visible" ) ==> 选择所有显示的元素

    - $( ":hidden" ) ==> 选择所有隐藏的元素

     **隐藏元素的方式**
        1. CSS ==> display: none
        2. width: 0; height: 0;
        3. visibility: hidden;
        4. opacity: 0;
        **说明：**如果元素**占据文档中一定的空间**，元素被认为是**可见**的，可见元素的宽高都大于 0 ，元素的 `visibility: hidden;` 和 `opacity: 0;` 被认为是可见的，因为他们仍然占据空间布局。不在文档中的元素被认为是不可见的，如果当他们被插入到文档中，jQuery 没有办法知道他们是否是可见的，因为元素可见性依赖于使用的样式。

9.  属性筛选选择器
    - $( "[attribute |= 'value']" ) ==> 选择指定属性值等于**给定字符串**或**以该字符串为前缀**（该字符串后跟一个连字符 '-'）的元素

    - $( "[attribute *= 'value']" ) ==> 选择指定属性具有**包含一个给定的子字符串**的元素（选择给定属性是包含某些值的元素）

    - $( "[attribute ~= 'value']" ) ==> 选择指定属性用**空格分隔**的值中包含一个给定值的元素

    - $( "[attribute = 'value']" ) ==> 选择指定属性是**给定值**的元素

    - $( "[attribute != 'value']" ) ==> 选择**不存在指定属性**，或者指定的属性**不等于给定值**的元素
    
    - $( "[attribute ^= 'value']" ) ==> 选择指定属性是以**给定字符串开始**的元素

    - $( "[attribute $= 'value']" ) ==> 选择指定属性是以**给定值结尾**的元素

    - $( "[attribute]" ) ==> 选择所有**具有指定属性**的元素，该属性是任意值

10. 表单元素选择器
    - $( ":input" ) ==> 选择所有 input textarea select 和 button 的元素

    - $( ":text" ) ==> 匹配所有文本框

    - $( ":password" ) ==> 匹配所有密码框

    - $( ":raido" ) ==> 匹配所有单选按钮

    - $( ":checkbox" ) ==> 匹配所有复选框

    - $( ":submit" ) ==> 匹配所有提交按钮
    
    - $( ":image" ) ==> 匹配所有图像域

    - $( ":reset" ) ==> 匹配所有重置按钮

    - $( ":button" ) ==> 匹配所有按钮

    - $( ":file" ) ==> 匹配所有文件域

    **说明：**
            1. 除了 input 筛选选择器，其余每个表单类别选择器都对应一个 input 元素的 type 值，所以其都可以用属性筛选选择器替换
            2. ` $( ":password" ) === $( "[type = password]" ) `

11. 表单对象属性筛选选择器

    - $( ":enabled" ) ==> 选取可用的表单元素

    - $( ":disabled" ) ==> 选取不可用的表单元素

    - $( ":checked" ) ==> 选取被选中的 <input> 元素

    - $( ":selected" ) ==> 选取被选中的 <option> 元素
    
    **说明：**
            1. 选择器适用于复选框和单选框，对于**下拉框**元素使用 **:selected** 选择器
            2. 在使用 :checked 的时候，**最佳实践是使用 input: checked** 

12. 子元素筛选选择器
    - $( ":first-child" ) ==> 选择所有父级元素下的第一个子元素

    - $( ":last-child" ) ==> 选择所有父级元素下的最后一个子元素

    - $( ":only-child" ) ==> 如果某个元素是其父元素下的唯一子元素，那么它就会被选中

    - $( ":nth-child(n)" ) ==> 选择所有父级元素下的第 n 个子元素，n 是索引值，从 1 开始

    - $( ":nth-last-child(n)" ) ==> 选择所有父级元素下的第 n 个子元素，计数从最后一个开始

13. 特殊选择器
    - this ==> 表示当前的上下文对象是一个 HTML 对象，可以调用 HTML 对象所拥有的属性和方法

    - $(this) ==> 代表上下文是一个 jQuery 的上下文对象，可以调用 jQuery 的方法和属性值

# jQuery 属性与样式
1.  .attr() 
    获取和设置元素属性（DOM操作）
    - .attr(传入属性名) ==> 获取属性值
    - .attr(属性名, 属性值) ==> 设置属性值
    - .attr(属性名, 函数值) ==> 设置属性的函数值
    - .attr(attribute) ==> 给指定元素设置多个属性值
    .attr({属性名1: "属性值1" , 属性名2: "属性值2", ...})

    **说明：Attribute 和 Property**
            1. Attribute 属性 ，  Property  特性
            2. Attribute 就是 DOM node 自带的属性（id、class、title），Property 是 DOM 元素作为对象，其附加的内容（nodeName、nodeType、tagName）

2. .removeAttr()
    为匹配的元素集合中的每个元素移除一个属性 attributeName

3. .html()
    获取或设置匹配元素的 HTML 内容
    - .html() ==> 获取集合中第一个匹配元素的 HTML 内容
    - .html(htmlString) ==> 设置每一个匹配元素的 HTML 内容
    - .html(function(index, oldhtml)) ==> 用来返回设置 HTML 内容的一个函数
    **说明：**
            .html() 方法内部使用的是 DOM 的 innerHTML 属性来操作的，这个操作是针对整个 HTML 内容（包括文本内容）

4. .text()
    得到匹配元素集合中的每个元素的文本内容的结合，包括他们的后代，或者设置匹配元素集合中每个元素的文本内容为指定的文本内容
    - .text() ==> 获取匹配元素集合中每个元素的合并文本，包括他们的后代
    - .text(textString) ==> 用来设置匹配元素内容的文本
    - .text(function(index, text)) ==> 用来返回设置文本内容的一个函数

    **说明：.html() 和 .text()**
            1. .html() 处理元素内容，.text() 处理文本内容
            2. html() 只能使用在 HTML 文档中，.text() 在 XML 文档和 HTML 文档都能使用
            3. 若处理对象只有一个子文本节点，则 .html 和 .text() 处理结果相同

5. .val()
    处理**表单元素**的值  
    - .val() ==> 获取匹配元素集合中第一个元素的当前值
    - .val(value) ==> 设置匹配的元素集合中每个元素的值
    - .val(function(){}) ==> 一个用来返回设置值的函数

    **说明：.html() 和 .text()**
            1. .val() 处理 select 元素，当没有选择项被选中时，它将返回 null 
            2. 若 select 元素有 multiple 属性，并且至少一个选择项被选中时， .val() 将返回一个数组，这个数组包含每个选中选择项的值
            3. .val() 方法多用来设置表单的字段的值

6. .addClass()
    动态增加 class 类名
    - .addClass(className) ==> 为每个匹配元素增加一个或多个类名
    - .addClass(function(index, currentClass){}) ==> 这个函数返回一个或更多用空格隔开的要增加的样式名
 
    **说明：**
            1. .addClass() 方法不会替换一个样式类名，它只是简单的添加一个类名到元素上
            2. 
    ```
    <script>
      $('div').addClass(function(index, className){
          if( className.indexOf(index) !== -1 ){
              $(this).addClass('red')
          }
       })
    </script>
    ```
7. .removeClass()
    删除全部或者指定的 class
    - .removeClass([className])  ==> 每个匹配的元素移除一个或对个用空格隔开的样式名
    - .removeClass(function(index, class)) ==> 返回一个或多个要被移除的样式名的函数

    **说明：**如果 .removeClass() 没有参数，则会移除所有的样式类

8. .toggleClass()
    在匹配的元素集合中的每个元素上添加或删除一个或多个样式类，取决于这个样式是否存在，即：如果存在就删除一个类，如果不存在就添加一个类
    - .toggleClass(className) ==> 切换类
    - .toggleClass(className,switch) ==> 一个布尔值，用来判断样式是否应该被添加或移除
    - .toggleClass(function(index, class, switch){})

9. .css()
    获取元素样式属性的计算值或者设置元素的 CSS 属性
    **获取**
    - .css( "propertyName" ) ==> 获取匹配元素集合中的第一个元素的样式属性的计算值
    - .css( "propertyNames" ) ==> 传递一组数组，返回一个对象结果

    **设置**
    - .css( 'propertyName', 'value' ) ==> 设置 CSS
    - .css( 'propertyName', function ) ==> 可以传入一个回调函数，返回取到对应的值进行处理
    - .css( 'properties ) ==> 可以传入一个对象，同时设置多个样式，用 ` { } ` 括起

    **说明：**
        1. 通过 .css() 方法设置的样式属性优先级要高于 .addClass() 方法
        2. 若是**静态结构**，确定了布局规则，使用 **.addClass()** 方法增加类
        3. 若是**动态的 HTML 结构**，在不确定布局或者经常要变化的情况下，使用 **.css()** 方法
        4. .addClass() 不能获取到指定样式的属性值，.css() 可以获取到指定的样式值