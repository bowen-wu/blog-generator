---
title: HTTP系列 -- AJAX 进阶
date: 2018-01-20 17:10:21
tags: HTTP
---
[相关代码链接](https://github.com/bowen-wu/Node.js-server-JSONP-AJAX/tree/master/AJAX-%E8%BF%9B%E9%98%B6)
# AJAX 设置响应 和 获取请求
### AJAX 设置请求 header
第一部分：` request.open( 'POST', 'http://www.baidu.com/search' ) `
第二部分：`  request.setRequestHeader( hander, value )  `
   ```
    request.setRequestHeader( 'Content-Type', 'application/x-www-form-urlencoded' )
    request.setRequestHeader( 'who', 'bowen' )
   ```
第四部分：` request.send( 'a=1&b=2' ) `
##### 请求
```
1. POST /search HTTP/1.1
2. Host: www.baidu.com
   Content-Type: application/x-www-form-urlencoded
   who: bowen
   Content-Length: 
   Cookie: 
3. 
4. a=1&b=2
```

### AJAX 获取响应 header
第一部分：` request.status ` 和 ` request.statusText `
第二部分：
```
request.getResponseHeader( headerName ) 
request.getAllResponseHeaders() 
```
第四部分：` request.responseText `
##### 响应
```
1. HTTP/1.1 200 ok
2. key: value
3. 
4. 数据
```
### Node.js（后端）设置响应 header
[详解](https://www.jianshu.com/p/19275eecb1b1)

# callback（回调）
callback 就是一个回调函数，如下图所示。
![callback 示例](http://upload-images.jianshu.io/upload_images/9617841-580ea1a7bb64c8af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
1. 给 window 添加一个方法 fn123 ，这个方法接受一个对象 options 。
2. 使用 window 下的方法 fn123 ，并传入一个对象，这个对象有一个方法 fn456 ，它是一个函数。
3. window 下的 fn123 接受到对象后将调用 fn456，将其 call back
4. fn456 中接受的 this 就是 window 下的方法中 call 时传的第一个参数，x 就是 call 时传的第二个参数


# 实现 jQuery.ajax
1. 首先回顾下 [window.jQuery](https://www.jianshu.com/p/3eb5f1c7937e)

2. 将 AJAX-基础 的[代码](https://github.com/bowen-wu/Node.js-server-JSONP-AJAX/tree/master/AJAX-%E5%9F%BA%E7%A1%80) 进行封装，封装为 window.jQuery.ajax，[代码地址](https://github.com/bowen-wu/Node.js-server-JSONP-AJAX/tree/da549df5393a229f1d3e878d27b491755e6e99bb)

3. 将封装的 window.jQuery.ajax 进行代码优化（给参数命名）。由于传入参数过多，将其参数设置为有结构的数据 ==> 对象，[代码地址](https://github.com/bowen-wu/Node.js-server-JSONP-AJAX/tree/80c6de9166b9efb60b2c34a5fb115aa9d0f61fcf)

4. 实现传入不定个参数的优化。既可以传入一个对象，也可以传入 url + 对象。[代码地址](https://github.com/bowen-wu/Node.js-server-JSONP-AJAX/tree/3ca84506652fa89c4f3854da77420d7f6094e240)。此时由于要使用 arguments ，所以不能使用箭头函数。

5. 优化代码，使用 ES6 解构赋值达到优化代码的目的，并增加 request.setRequestHeader() 功能。[代码地址](https://github.com/bowen-wu/Node.js-server-JSONP-AJAX/tree/5118accb4c21f0ed2b8da6f2c5677867d238bf51)。

6. 如果不实现传入不定个参数，还可以进一步使用 ES6 解构赋值优化。[代码地址](https://github.com/bowen-wu/Node.js-server-JSONP-AJAX/tree/88436fc2378cec3a814a96cc3260a5de133038da/AJAX-%E8%BF%9B%E9%98%B6)

7. 使用 Promise 规范进行优化，` return new Promise(function(resolve,reject){}) `，成功 === 调用 resolve，失败 === 调用 reject，之后在调用时使用 ` .then ` 方法。[代码地址](https://github.com/bowen-wu/Node.js-server-JSONP-AJAX/tree/1ea0e7728b98f008f077e7b7f77b2eb80ec77e6b/AJAX-%E8%BF%9B%E9%98%B6)

8. Promise 实现原理
    ```
    window.Promise = function(fn){
      // ......
       return {
            then: function(){}
       }
    }
    ```