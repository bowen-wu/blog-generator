---
title: HTTP系列 -- AJAX 基础
date: 2018-01-20 17:01:25
tags: HTTP
---
[参考代码](https://github.com/bowen-wu/server-Node.js-demo/tree/master/AJAX-%E5%9F%BA%E7%A1%80)
# 概述
AJAX（Async JavaScript And XML）：异步 JavaScript 和XML，远程编程脚本，使 JavaScript 能够在 Web 服务器上运行的程序通信，它是一种无需重新加载整个网页的情况之下能够更新部分网页的技术。
### 条件
1. 使用 XMLHttpRequest 发请求
2. 服务器返回 XML / **JSON** 格式的**字符串**
3. JS 解析 XML / **JSON**，并更新局部页面
### 说明
HTTP 的 path（路径）都是绝对路径


# 如何发送请求
### form 表单
使用 form 表单可以发请求，但是会刷新页面或新开页面
    ![form 标签创建](http://upload-images.jianshu.io/upload_images/9617841-a4cd551b880d7edc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![form 标签发送请求](http://upload-images.jianshu.io/upload_images/9617841-fa120004a4c5d9b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![form 标签发送 GET 请求](http://upload-images.jianshu.io/upload_images/9617841-9f0cfec4a4ad3a6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![form 标签发送 POST 请求](http://upload-images.jianshu.io/upload_images/9617841-13288ee474125f39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### a 标签
使用 a 标签可以发 GET 请求，但是也会刷新页面或新开页面
![a 标签创建](http://upload-images.jianshu.io/upload_images/9617841-f8081f7df640b734.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![a 标签发送 GET 请求](http://upload-images.jianshu.io/upload_images/9617841-1b8aad5535592eed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### img 标签
使用 img 标签可以发 GET 请求，但是只能以图片的形式展示
![img 标签创建](http://upload-images.jianshu.io/upload_images/9617841-419575e6ec290994.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![img 标签发送 GET 请求](http://upload-images.jianshu.io/upload_images/9617841-78c17819094b4e87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### link 标签
使用 link 标签可以发 GET 请求，但是只能以 CSS、favicon 的形式展示
![link 标签创建](http://upload-images.jianshu.io/upload_images/9617841-67c1f5fea5f0cd93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![link 标签发送 GET 请求](http://upload-images.jianshu.io/upload_images/9617841-396bbc64663e8d73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### script 标签（JSONP）
使用 script 标签可以发 GET 请求，但是只能以脚本的形式运行
![script 标签创建](http://upload-images.jianshu.io/upload_images/9617841-b11d2c9d39d32347.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![script 标签发送 GET 请求](http://upload-images.jianshu.io/upload_images/9617841-16b27a15892f6486.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# XMLHttpRequest 对象
`XMLHttpRequest` 是一个 API，它为客户端**提供了在客户端和服务器之间传输数据的功能**。它提供了一个通过 URL 来获取数据的简单方式，并且不会使整个页面刷新。这使得网页只更新一部分页面而不会打扰到用户。`XMLHttpRequest` 在 [AJAX](https://developer.mozilla.org/zh-CN/docs/AJAX) 中被大量使用。

![挂载在 window 下的 XMLHttpRequest 对象](http://upload-images.jianshu.io/upload_images/9617841-15f0d81c590baedd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 属性
##### onreadystatechange
- 类型：function
- JavaScript函数对象，当readyState属性改变时会调用它。

##### readyState
类型：Number
显示 XMLHttpRequest  代理当前所处的状态，值：
- 0 ==> 代理被创建，但尚未调用 open() 方法
- 1 ==> open() 方法已经被调用
- 2 ==> send() 方法已经被调用，并且头部和状态已经可获得
- 3 ==> 下载中，requestText 属性已经包含部分数据
- 4 ==> 下载操作以完成

##### responseText
- 类型：String
响应的文本

##### responseType
设置响应类型，值：
- "" ==> 字符串（默认值）
- "arraybuffer"
- "blob"
- "document"
- "json" ==> JavaScript 对象，解析自服务器传递回来的JSON 字符串
- "text" ==> 字符串

##### status
请求的响应状态码
[详见](https://www.jianshu.com/p/036df537e96d)

##### statusText
请求的响应状态信息,包含一个状态码和原因短语（"200 OK"） 

### 方法
##### 1. open()
初始化一个请求。该方法用于JavaScript代码中
```
let request = new XMLHttpRequest
request.open( method, url, async )
```
###### 参数
- methos ==> 请求所使用的HTTP方法
- url ==> 该请求所要访问的 URL / 路径
- async ==> 默认为true（异步），表示是否执行异步操作

##### 2. setRequestHeader()
给指定的HTTP请求头赋值.在这之前,你必须确认已经调用 ` open() `
```
let request = new XMLHttpRequest
request.open( 'POST', '/xxx' )
request.setRequestHeader( header, value )
```
**request.setRequestHeader( 'Content-type' , 'application/x-www-form=urlcoded' )**
###### 参数
- header ==> 将要被赋值的请求头名称
- value ==> 给指定的请求头赋的值

##### 3. send()
发送请求。如果该请求是异步模式(默认)，该方法会立刻返回。相反，如果请求是同步模式，则直到请求的响应完全接受以后，该方法才会返回

##### getAllResponseHeaders()
返回所有响应头信息(响应头名和值)，如果响应头还没接受,则返回null

##### getResponseHeader()
返回指定的响应头的值，如果响应头还没被接受，或该响应头不存在，则返回null






# JSON 对象
**`JSON`**对象包含用于解析 JSON 的方法，并将值转换为 JSON。它不能被调用或者作为构造函数
### 方法
##### JSON.parse()
解析一个JSON字符串，可选地转换生成的值及其属性，并返回值

##### JSON.stringify()
返回与指定值相对应的一个JSON字符串，可选地仅包含某些属性或以用户定义的方式替换属性值


# 原生 JavaScript 发送 AJAX 请求
```
let request = new XMLHttpRequest()
//open() 方法有第三个参数，true 为异步（默认），false为同步
request.open( 'GET', '路径' )   // 配置 request 

//request.open( 'POST', '路径' )  
// 配置 request ，如果是 POST 方法，必须设置 setRequestHeader
//request.setRequestHeader( 'Content-type' , 'application/x-www-form=urlcoded' )

request.send()
request.onreadystatechange = () =>{
    if( request.readyState === 4 ){ //请求响应都完毕了
        if( request.status >= 200 && request.status < 300 ){
            console.log( '说明请求成功' )
            let string = request.responseText
            // 把符合 JSON 语法的字符串转换成 JS 对应的值
            let object = window.JSON.parse( string )
        }
    }
}
```

**说明：**
- HTTP 第四部分永远是字符串，只不过是符合 JSON 对象语法的字符串（解析 JSON.parse()）

- 如何判断代码执行时长
     ```
    console.time()
    要判断的代码
    console.end()
    ```

- 如何判断同步异步
   ```
    console.log( 'start' )
    要判断的代码
    console.log( 'end' )
    ```


# JSON
JSON（JavaScript Object Notation）：JavaScript 对象表示法，它是存储和交换文本信息的语法，类似 XML，它采用键值对的方式来组织，易于人们阅读和编写，同时也易于机器解析和生成。**它是一门新的语言**。

### JSON 语法
[JSON 铁轨图](https://www.json.org/)
JSON 中有 String、null、Number、true、false、Array 和 Object

### JSON 和 JavaScript 的区别
JSON 和 JavaScript 是两门语言，JSON 是抄袭的 JavaScript，两者区别主要体现在：
- JSON 没有 undefined、function 和 Symbol
- JSON 必须使用双引号
- JSON 没有变量
- JSON 中对象仅仅是一个哈希

# 同源策略
### 概述
- 当一个资源从与该资源本身所在的服务器不同的域或端口**发送 AJAX** 请求一个资源时，资源会发起一个跨域 HTTP 请求，出于安全原因，浏览器限制从脚本内发起的跨源HTTP请求

- 只有 AJAX 被限制，其他的都可以（img、form、link 和 script）

- 原页面用 form 提交到另一个域名之后，原页面的脚本无法获取新页面的内容，所以浏览器认为这是安全的。而 AJAX 是可以读取相应内容的，因此浏览器将阻止你这样做

- 如果使用 AJAX 向其他源发送请求，会成功，但是报错（request.status === 0）

- AJAX 请求已经发送出去了，却拿不到响应
 request.status === 0

- 同源策略本质：一个域名的 JS ，在未经允许的情况下，不得读取另一个域名的内容，但是浏览器并不阻止向另一个域名发送请求。**只有协议、域名和端口一模一样才允许发送 AJAX 请求**

### 为什么要使用同源策略
**安全**，如果没有同源策略，AJAX 就可以向另一个域名发送 GET / POST 请求，GET 请求可以获取信息，POST 请求可以直接转账，那样互联网就没有隐私可言

### 跨域
1. JSONP：但是 JSONP 有局限，不能发送 POST 请求

2. CORS（Cross-Orogin Resource Sharing 跨域资源共享）：请求的域名的**后端**需要在相应的接口添加**响应头**，此时就可以发送 POST 请求
    ``` 
    response.setHeader( "Access-Control-Allow-Origin", "http://......" ) 
    ```
