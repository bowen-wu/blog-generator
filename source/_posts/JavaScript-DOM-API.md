---
title: JavaScript DOM API
date: 2018-01-08 18:37:51
tags: JavaScript
---

# 概述
1. DOM（Document Object Model）文档对象模型，指的是 XML 文档，需要说明的是，HTML 是 XML 的衍生品，如果要操作 HTML 可以使用关于 XML 的文档 DOM。

2. JS 中的对象继承自 Object ，页面中的对象继承自 Node 函数。
Model 将页面中的 Document（html、head、body）和内存中的 Object 一一映射。

3. DOM 就是把文档变成对象的一种模型。

4. 内存中的 Object 是一个个 node （构造函数），其包括：
    - Element Node（元素节点）
    - Text Node（文本节点）
    - Comment Node（注释节点）
    - Document Node（文档节点）

5. DOM 功能：页面中的节点通过构造函数构造出对应的对象
    ```
    Element.__proto__ === Node.prototype
    Document.__proto__ === Node.prototype
    ```

# API
### Node 接口
**Node 派生自Object**
Node是一个接口，许多DOM类型从这个接口继承，并允许类似地处理（或测试）这些各种类型。

##### 属性
- ` .childNodes`  ==>    返回包含指定节点的子节点的集合，该集合为即时更新的集合（live collection）

- ` .firstChild`  ==>     只读属性返回树中节点的第一个子节点，如果节点是无子节点，则返回 null

- ` .lastChild`  ==>     只读属性，返回当前节点的最后一个子节点。如果没有子节点，则返回 null

- ` .nextSibling`  ==>     只读属性，返回其父节点的 `childNodes` 列表中紧跟在其后面的节点，如果指定的节点为最后一个节点，则返回 `null`。**可能会获取到文本节点**

- ` .previouSibling`  ==>     返回当前节点的前一个兄弟节点,没有则返回null。**可能会获取到文本节点**

- ` .nodeName ` ==> 返回当前节点的节点名称（**svg 标签返回小写字母，其余返回大写字母**）

- ` .nodeValue ` ==> 返回或设置当前节点的值

- ` .nodeType ` ==> 只读属性，表示的是该节点的类型
    - 元素节点（Node.ELEMENT_NODE） ==> nodeType === 1
    - 文本节点（Node.TEXT_NODE） ==> nodeType === 3
    - 注释节点（Node.COMMENT_NODE） ==> nodeType === 8
    - 文档节点（Node.DOCUMENT_NODE） ==> nodeType === 9
    - 描述文档类型的 `DocumentType`节点（Node.DOCUMENT_TYPE_NODE） ==> nodeType === 10
    - 一个 `DocumentFragment` 节点（Node.DOCUMENT_FRAGMENT_NODE） ==> nodeType === 11（**使用 `DocumentFragment` 进行性能优化**）

- ` .parentElement`  ==>     返回当前节点的父元素节点,如果该元素没有父节点,或者父节点不是一个元素节点.则 返回null

- ` .parentNode`  ==>     返回指定的节点在DOM树中的父节点

- ` .innerText ` ==> 表示一个节点及其后代的“渲染”文本内容的属性
- ` .textContent ` ==> 表示一个节点及其后代的文本内容
**说明：` .innerText ` 和 ` .textContent ` 的区别**
    - **`textContent`** 会获取所有元素的内容，包括 `<script>` 和 `<style>`元素，然而 **innerText **不会
    - innerText意识到样式，并且不会返回隐藏元素的文本，而textContent会
    - 由于 `innerText` 受 CSS 样式的影响，它会触发重排（reflow），但`textContent` 不会


##### 方法
- ` .appendChild(  ); `  ==> 添加子元素

- ` .cloneNode( deep ); `  ==> 返回调用该方法的节点的一个副本
    **参数deep** 表示是否采用深度克隆，如果为 true，则该节点的所有后代节点也都会被克隆，如果为 false，则只克隆该节点本身。默认为 false。

- ` Node.contains() ` ==> 返回一个布尔值，表示传入的节点是否为该节点的后代节点。

- ` .insertBefore( newDiv, currentDiv ); `  ==> 在元素前插入元素

- ` .removeChild( child ); `  ==> 移除一个节点（在页面中移除，但是仍在内存中）

- ` .replace( newChild, oldChild ); `  ==> 用指定的节点替换当前节点的一个子节点，并返回被替换掉的节点。

- ` .hasChildNodes(  ); `  ==> 返回一个布尔值,表明当前节点是否包含有子节点

- ` .isEqualNode(  ); `  ==> 可以判断两个节点是否相等

- ` .normalize() ` ==> 将当前节点和它的后代节点”规范化“（normalized）。在一个"规范化"后的DOM树中，不存在一个空的文本节点，或者两个相邻的文本节点


### Document 接口
**Document** 接口提供了一些在浏览器服务中作为页面内容入口点而加载的一些页面，也就是 DOM 树

##### 属性

- ` .body ` ==> 返回当前文档的 `<body>` 元素

- ` .characterSet ` ==> 只读属性返回当前文档的字符编码

- ` .childElementCount ` ==> 只读属性，返回一个无符号长整型数字，表示给定元素的子元素数

- ` .children`  ==>   只读属性，返回 一个Node的子`elements` ，是一个动态更新的 `HTMLCollection`

- ` .documentElement ` 是一个会返回文档对象（`document`）的根元素（如HTML文档的 <html> 元素）

- ` .domain ` ==> 获取/设置当前文档的原始域部分, 用于同源策略

- ` .firstElementChild ` ==> 只读属性，返回对象的第一个孩子 `Element`， 如果没有子元素，则为null

 - ` .lastElementChild ` ==> 只读属性，返回对象的最后一个孩子`Element`，如果没有子元素，则返回null

- ` .readyState ` ==> 描述了文档的加载状态

- ` .referrer ` ==> 返回跳转或打开到当前页面的那个页面的URI

- ` .onkeypress ` ==>  用来获取或设置当前元素的keypress事件的事件处理函数

- ` .onclick `  ==>     返回当前元素的 click 事件句柄代码

- ` .onmouseover `  ==>    当指针设备移动到存在监听器的元素或其子元素的时候，mouseover事件就会被触发。（冒泡）

- ` .onmouseout `  ==>     当指针设备移出存在监听器的元素或其子元素的时候，mouseover事件就会被触发。（冒泡）

- ` .onmouseenter `  ==>     当定点设备（通常指鼠标）移动到元素上时就会触发 mouseenter 事件.类似 `mouseover` ，它们两者之间的差别是 `mouseenter `**不会冒泡**

- ` .onmouseleave `  ==>     当定点设备（通常指鼠标）移出元素时就会触发 mouseleave 事件.类似 `mouseout ，它们两者之间的差别是 `mouseleave `**不会冒泡**


###### PC端：
- ` .onmousedown `  ==>    用来获取或设置当前元素的mousedown事件的事件处理函数

- ` .onmousemove `  ==>    用来获取或设置当前元素的mousemove事件的事件处理函数。（当用户在当前元素上移动鼠标时会触发mousemove事件.）。mdn上有 1.显示"提示层"时onmousemove的用法.  2.在进行拖拽操作时onmousemove的用法

- ` .onmouseup `  ==>     用来获取或设置当前元素的mouseup事件的事件处理函数

###### 移动端：
- ` .ontouchstart`  ==>    当触点与触控设备表面**接触**时触发 ontouchstart  事件

- ` .ontouchmove`  ==>    当触点与触控设备表面**移动**时触发 ontouchstart  事件

- ` .ontouchend`  ==>    当触点**离开**触控平面时触发touchend事件.
特性检测：如果设备不支持 touch 事件，那么元素的 ontouchstart 为undefined ，表示未定义。如果设备支持 touch 事件，那么元素的 onstarttouch 为 null ，表示初始化了

- ` .onerror `  ==>     错误(`error`)事件的一个事件处理程序(event handler)。针对各种目标的不同类型的错误触发了 Error 事件：
    *   当**JavaScript运行时错误**（包括语法错误）发生时，`window`会触发一个`ErrorEvent`接口的`error`事件，并执行`window.onerror()`。
    ```
     window.onerror = function(message, source, lineno, colno, error) { ... }
    ```
    *   当一项资源（如`<img>`或`<script>`）**加载失败**，加载资源的元素会触发一个`Event`接口的`error`事件，并执行该元素上的`onerror()`处理函数。这些error事件不会向上冒泡到window，不过（至少在Firefox中）能被单一的`window.addEventListener`捕获。
    ```
    element.onerror = function(event) { ... }
    ```

- ` onscroll ` ==> 当用户滚动某个元素的内容时 scroll 事件将会被触发

- ` scrollTo ` ==> 滚动到文档中的某个坐标

##### 方法

- ` .querySelectorAll() ` ==> 返回与指定的选择器组匹配的文档中的元素列表 (使用深度优先的先序遍历文档的节点)。返回的对象是 NodeList 

- ` .querySelector() ` ==> 返回文档中匹配指定的选择器组的第一个元素(使用深度优先先序遍历文档的节点 | 并且通过文档标记中的第一个元素，并按照子节点数量的顺序迭代顺序节点)。 如果没有找到匹配元素，则返回 null，如果找到多个匹配元素，则返回第一个匹配到的元素。CSS 伪类不会返回任何元素 //直接可以传入带有 # 的 href ，也会返回正确的标签。参数是一个字符串，包含一个或是多个 CSS 选择器 ，多个则以逗号分隔
- ` .getElementById() ` ==>  

- ` .getElementsByName() ` ==>  

- ` .getElementsByClassName() ` ==>  

- ` .getElementsByTagName() ` ==>  

- ` .createElement(); ` ==> 创建由tagName 指定的HTML元素

- ` .createTextNode(); ` ==> 创建一个新的文本节点

- ` .open() ` ==> 打开一个要写入的文档。
- ` .write() ` ==> 将一个文本字符串写入由 ` document.open() ` 打开的一个文档流。
    **注意：** 因为 ` document.write ` 写入**文档流**，在关闭(已加载)的文档上调用 ` document.write ` 会自动调用 ` document.open `，**这将清除该文档**
- ` .close() ` ==> 关闭向当前文档的数据写入

### Element 接口
**Element**（元素）接口是 ` Document ` 的一个对象. 这个接口描述了所有相同种类的元素所普遍具有的方法和属性

##### 属性

- ` .clientHeight `  ==>  只读属性，表示元素的内部高度，以像素计。该属性包括内边距，但不包括垂直滚动条（如果有）、边框和外边距

- ` .clientWidth `  ==>  只读属性，表示元素的内部宽度，以像素计。该属性包括内边距，但不包括垂直滚动条（如果有）、边框和外边距

- ` .clientLeft `  ==>  只读属性，表示一个元素的左边框的宽度，以像素表示。不包括左外边距和左内边距

- ` .clientTop `  ==>  只读属性，表示一个元素顶部边框的宽度，以像素表示。不包括顶部外边距或内边距

- ` .innerHTML  `  ==>  设置或获取描述元素后代的HTML语法

- ` .scrollHeight `  ==>  只读属性，表示一个元素的内容高度，包括由于溢出导致的视图中不可见内容。包括元素的padding，但不包括元素的border和margin

- ` .scrollWidth `  ==>  只读属性，表示一个元素的内容宽度，包括由于溢出导致的视图中不可见内容。包括元素的padding，但不包括元素的border和margin

- ` .scrollLeft `  ==>  读取或设置元素滚动条到元素左边的距离

- ` .scrollTop `  ==>   读取或设置元素滚动条到元素顶部的距离

- ` Element.className `  ==>  获取并设置指定元素的class属性的值。可以是由**空格**分隔的多个class属性值

**Element.classList**：只读属性，返回一个元素的类属性的实时 `DOMTokenList`集合。
**方法：**
- ` .add()`  ==>    添加指定的类值。如果这些类已经存在于元素的属性中，那么它们将被忽略。
- ` remove()`  ==>   删除指定的类值。 
- ` contains()`  ==>   检查元素的类属性中是否存在指定的类值。
- ` item( Number )`  ==>   按集合中的索引返回类值。
- ` toggle( String )`  ==>   当只有一个参数时：切换 class value; 即如果类存在，则删除它并返回false，如果不存在，则添加它并返回true。
    ```
    // div是具有class =“foo bar”的<div>元素的对象引用
  div.classList.remove("foo");

  // 如果visible被设置则删除它，否则添加它
  div.classList.toggle("visible");

  //添加或删除多个类
  div.classList.add("foo","bar");
    ```

##### 方法

- ` .getAttribute() ` ==>  返回元素上一个指定的属性值。如果指定的属性不存在，则返回  null 或 "" （空字符串）
**注意：**
    1. a.href ==> 获取 a 的 href，带有 http://，是被浏览器处理过了
    2. a.getAttribute() ==> 获取 a 的 href ，不带有 http://，用户的写入

- ` .setAttribute() ` ==> 设置指定元素上的一个属性值。如果属性已经存在，则更新该值; 否则将添加一个新的属性用指定的名称和值。

- ` .removeAttribute() ` ==> 从指定的元素中删除一个属性。


### HTMLElement 接口
HTMLElement 接口表示所有的 HTML 元素。一些 HTML 元素直接实现了 HTMLElement 接口，其它的间接实现 HTMLElement 接口

##### 属性

- ` HTMLElement.offsetHeight `  ==>  只读属性，它返回该元素的像素高度，包含该元素的垂直内边距（padding）和边框（border）

- ` HTMLElement.offsetWidth `  ==>   只读属性，它返回该元素的像素宽度，包含该元素的垂直内边距（padding）和边框（border）

- ` HTMLElement.offsetTop ` ==> 为只读属性，它返回当前元素相对于其 offsetParent 元素的顶部的距离

- ` HTMLElement.offsetLeft ` ==> 为只读属性，返回当前元素左上角相对于  HTMLElement.offsetParent 节点的左边界偏移的像素值。

- ` HTMLElement.offsetParent `  ==>  只读属性，返回一个指向最近的包含该元素的定位元素。


##### 方法
- ` HTMLElement.blur() `  ==>  移除当前元素所获得的键盘焦点

- ` HTMLElement.click `  ==>  模拟鼠标左键单击一个元素

- ` HTMLElement.focus `  ==>  设置指定元素获取焦点

