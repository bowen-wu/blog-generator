---
title: HTTP系列 -- JSONP
date: 2018-01-20 16:56:11
tags: HTTP
---
[参考代码](https://github.com/bowen-wu/server-Node.js-demo/tree/master/JSONP)
# 概述
数据库是用来长久储存数据的。

## 发送请求
#### ` form ` 表单提交
缺点：刷新页面 + 用户后退 + 用户刷新 = 数据变动

#### ` form ` + ` iframe ` 提交
` form ` 表单提交到 ` iframe ` 中，页面不会刷新。
缺点：用户刷新页面  ==>  数据变动

#### JS动态创建 ` img ` 标签
    ```
    let img = document.createElement('img');
    img.src = '路径'
    img.onload = function(){}  ==>  HTTP状态码
    img.onerror = function(){}  ==>  HTTP状态码
    ```
通过路径，在 Nodejs 中设置成功或者失败，之后返回状态码，前端通过返回的HTTP状态码，进而来判断成功失败。

#### 动态创建 ` script ` 标签
    ```
    let script = document.createElement('script');
    script.src = '路径';
    document.body.appendChild(script);
    script.onload = function(){}
    script.onerror = function(){}
    ```
- 通过设置响应头中的类型进而来确定返回内容的格式（第四部分），从而保证返回的是 JavaScript 代码。
-  当创建  ` script ` 标签插入 ` body ` 后，返回的内容会立即执行，之后再去执行 ` onload ` 和 ` onerror ` 事件，所以说  ` onload ` 可以进行删除， 之后将处理逻辑交给后端（处理逻辑中应该包括使用完 ` script ` 标签之后立刻删除），但是此时前后端耦合。
- ` script ` 的 ` src ` 属性可以访问其他地址

# JSONP
动态创建 ` script ` 并调用前端传给后端的 callback 技术
### 版本一
#####Nodejs：
```
response.write(` callback.call(undefined,' success ') `);
```
    
##### JS:
```
window.callback = function(){}  //返回之后进行处理
script.src = http://....../路径?callback = xxx
```
### 版本二
##### Nodejs
```
response.write(` ${ query.callback }.call(undefined,{
    "success": true,
    "left": ${ newAmount }
}) `)
```
返回的数据是 JSON 数据
**特点：**
- 请求方创建 ` script ` ，` src ` 属性指向响应方
- 响应方根据查询参数，构造形如 xxx.call(undefined,"传给前端的数据") 这样的响应。

# JSONP
### 什么是 JSONP
请求方：前端（浏览器）
响应方：后端（服务器）
1. 请求方创建 ` script ` ，` src ` 指向响应方，同时传入一个查询参数 ?callback = 随机数
2. 响应方根据查询参数 callback ，构造形如 ` 随机数.call(undefined,"传给前端的数据") ` 这样的响应
3. 浏览器接收到响应，就会执行 ` 随机数.call(undefined,"传给前端的数据") ` 
4. 请求方获取到数据
### 完整代码
##### JS
```
button.addEventlistener('click', (e) => {
    let script = document.createElement('script');
    let functionName = 'bowen' + parseInt(Math.random()*100000,10);
    window[ functionName ] = function(result){
        if( result === 'success' ){
            // do something
        }else{
            // do something
        }
    }
    script.src = 'http://....../路径?callback = ' + functionName
    document.body.appendChild( script );
    script.onload = function(event){
        event.currentTarget.remove();
        delete window[ functionName ]
    }
    script.onerror = function(){
        alert('fail');
        event.currentTarget.remove();
        delete window[ functionName ]
    }
})
```
##### jQ:
```
button.addEventListener('click',(event) => {
    $.ajax({
        url: 'http://....../路径'，
        dataType: 'jsonp',
        success: function( response ){
            // do something
        }
    })
})
```

### 套路：为什么 JSONP 不支持 POST 请求
JSONP 是动态创建 ` script ` 标签 + 传入 callback 查询参数，` script ` 标签，只能发送 GET 请求，不能使用 POST。

### 相关知识点
如果后端没有写 
```
response.write(` ${ query.callback }.call(undefined,{
    "success": true,
    "left": ${ newAmount }
}) `)
```
前端虽然有响应内容，但是使用不到