---
title: JavaScript事件处理程序
date: 2017-12-22 23:28:10
tags: JavaScript
---
# 事件
> 事件就是文档或浏览器窗口中发生的一些特定的交互瞬间。

# 事件流
> 事件流描述的是从页面中接收事件的顺序
**IE**的事件流是事件冒泡流，**DOM**的事件流是事件捕获流
### 事件冒泡
> 事件的开始是由最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）
如果你点击`div`,那么在`div`上接收到`click`事件，之后传播到`div`的父元素，`div`父元素接收到`click`事件，再向上传播至`div`的爷爷元素，`div`爷爷元素接收到`click`事件，再向上传播，直到`document`。
### 事件捕获
> 不太具体的节点应该更早的接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于在时间到达预定目标之前捕获它
如果你点击`div`,那么首先在`document`上接收到`click`事件，之后向下捕获到`div`的爷爷元素，`div`爷爷元素接收到`click`事件，再向下捕获至`div`的父元素，`div`父元素接收到`click`事件，再向下捕获，直到`div`。
### DOM事件流
> "DOM2级事件"规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段
先捕获在冒泡，DOM树中每个元素都接收两次事件

# 事件处理程序( event handler )
响应事件的函数，当有事件触发了之后，作出的回应。
### HTML事件处理程序
```
<script>
    function showMessage () {
        alert("Hello World!");
    }
</script>
<input type = 'button' value = 'Click Me' onclick = 'showMessage()' />
```
**注意：**此时的`onclick = 'showMessage()'`中的 'showMessage' 带有 '()'，因为`onclick = '要执行的代码'`，一旦用户点击浏览器就`eval('要执行的代码')`。
### DOM2级事件处理程序
`addEventListener()`添加事件处理程序（事件监听）
    参数：
        要处理的事件名
        事件处理函数
        布尔值[可选]：如果是`true`，则表示在捕获阶段调用事件处理程序，如果是`false`或者未设置，表示在冒泡阶段调用事件处理程序
`removeEventListener()`删除事件处理程序（事件监听）
    参数：
        要处理的事件名
        事件处理函数
        布尔值[可选]：如果是`true`，则表示在捕获阶段调用事件处理程序，如果是`false`或者未设置，表示在冒泡阶段调用事件处理程序
通过`addEventListener()`添加的事件处理程序只能通过`removeEventListener()`来删除，并且移除时传入的参数与添加处理程序时使用的参数相同，所以这也就意味着通过`addEventListener()`添加的匿名函数将无法被移除，所以我们一定要给`addEventListener()`的事件处理函数命名，从而确保我们能够将其移除。
**优点：**可以为同一个元素添加多个事件处理程序
### IE事件处理程序
`attachEvent()`添加事件处理程序（事件监听）
    参数：
        要处理的事件名
        事件处理函数  
`detachEvent()`删除事件处理程序（事件监听）
    参数：
        要处理的事件名
        事件处理函数
由于 IE8 及更早的版本只支持冒泡，所以通过`attachEvent()`添加的事件处理程序都会被添加到冒泡阶段
**注意：**在使用`attachEvent()`方法的情况下，事件处理程序会在全局作用域中运行，此时`this === window`.
## 跨浏览器的事件处理程序
使用能力检测。并且要保证处理事件的代码能在 DOM 和 IE 下一致地运行，只需要关注*冒泡阶段*
```
function addHandler ( element,type,handler ) {
    if ( element.addEventListener ){
        element.addEventListener( type,handler,false );
    } else if ( element.attachEvent ){
        element.attachEvent( 'on' + type,handler );
    } else {
        element[ 'on' + type ] = handler;
    } 
}

function removeHandler ( element,type,handler ) {
    if ( element.removeEventListener ){
        element.removeEventListener( type,handler,false );
    } else if ( element.detachEvent ){
        element.detachEvent( 'on' + type,handler );
    } else {
        element[ 'on' + type ] = null;
    } 
}
```

**注意：**当在目标元素上既有捕获阶段调用的事件处理程序，又有冒泡阶段调用的事件处理程序，则按照代码书写顺序依次调用。
```
btn.addEventListener( 'click',function(){
    console.log( '捕获阶段' )；
},true );

btn.addEventListener( 'click',function(){
    console.log( '冒泡阶段' )；
},false );
```
以上代码依次打印出
    **捕获阶段**
    **冒泡阶段**

```
btn.addEventListener( 'click',function(){
    console.log( '冒泡阶段' )；
},false );

btn.addEventListener( 'click',function(){
    console.log( '捕获阶段' )；
},true );
```
以上代码依次打印出
    **冒泡阶段**
    **捕获阶段**

    