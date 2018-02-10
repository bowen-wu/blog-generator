---
title: HTTP系列 -- Session + Storage + Cache-Control + ETag + Cookie
date: 2018-02-10 08:28:18
tags: HTTP
---
# 概述
本文主要讲述 Session + Storage + Cache-Control + ETag + Cookie 这五者的作用及区别

# Session
首先通过代码认识一下 Session。之前我们说 Cookie 可以存储我们的一些信息，但是由于用户在浏览器中可以对 Cookie 进行操作，显然这不是我们想要的，所以 Session 应运而生，Session **解决了 Cookie 不安全的痛点**
#### code
我们在内存中开辟一个空间，用来存储 Session
```
let sessions = {}
```
当用户登录成功时
```
let sessionId = Math.random() * 1000000
sessions[ sessionId ] = { key: value }  // 表示存储的用户信息
response.setHeader( 'Set-Cookie', `sessionId = ${ sessionId }` )  // Cookie: 'sessionId = 随机数'
```
当此用户访问首页时，遍历 Cookie，将所有 Cookie 存储到一个 hash（哈希表）中，之后
```
let mySession = sessions[ hash.sessionId ]
let username
if( mySession ){
    username = mySession.用户信息  // 用户信息表示 sessions 中的{ key: value }
}
```
###### Session 特点
1. 服务器通过 Cookie（` sessionId = ${ sessionId } `） 将 SessionId（随机数）发给浏览器
2. 服务器有一块内存保存了所有的 Session（哈希表）
3. 当浏览器访问服务器时，服务器读取 SessionId
4. 服务器通过 SessionId 可以得到对应用户的隐私信息
5. 用户每次登录都会设置一个 SessionId，并且 SessionId 不保存在服务器中

# Storage
作为 Web Storage API 的接口（HTML5），Storage 提供了访问特定域名下的会话存储（session storage）和本地存储（local storage）的功能，例如：增删改查存储的数据项。Storage 与 HTTP 无关，它是浏览器上的哈希表，Storage 文件存储在本地的一个文件夹中
- ` window.sessionStorage ` ==> 操作**一个域名**的会话存储（session storage）
- ` window.localStorage ` ==> 操作**一个域名**的本地存储（local storage）
#### API
- ` Storage.setItem() ` ==> 接收一个键名和值作为参数，把键值对添加到存储中，如果键名存在，则更新其对应的值 

- ` Storage.getItem() ` ==> 接收一个键名作为参数，返回键名对应的值

- ` Storage.removeItem() ` ==> 接收一个键名作为参数，并把该键名从存储中删除

- ` Storage.clear() ` ==> 清空存储中的所有键名

###### 对象的存储
```
localStorage.setItem( 'object', { name: 'obj' } )  // object  [ object Object ]
```
![存储对象](http://upload-images.jianshu.io/upload_images/9617841-4215d1335bb982ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
浏览器会将 ` { name: 'obj' } ` 转化为字符串即 ` ({ name: 'obj' }).toString `，所以当我们存储对象时，使用 JSON ，即
```
localStorage.setItem( 'object', JSON.stringify({ name: 'obj' }))
```

#### localStorage 
###### 使用场景
记录是否提示过用户 + 记录一些不敏感的信息，常见新手引导界面
```
let already = localStorage.getItem( 'isGuide' )
if( !already ){
    // 开启引导
    localStorage.setItem( 'isGuide', true )
}
```
###### 特点
1. localStorage 与 HTTP 无关，所以 HTTP 不会带上 localStorage 的值
2. 每个域名的 localStorage 有最大存储量，因浏览器而异
3. 只有相同域名的页面才能互相读取 localStorage
4. **localStorage 永久有效，除非用户清除**

#### sessionStorage
###### 特点
1. sessionStorage 与 HTTP 无关，所以 HTTP 不会带上 sessionStorage 的值
2. 每个域名的 sessionStorage 有最大存储量，因浏览器而异
3. SessionStorage 只在同一浏览器窗口中共享
4. **sessionStorage 在用户关闭页面后就会失效**

# Cache-Control
Cache-Control 通用消息头被用于在 HTTP 请求和响应中通过指定指令来实现缓存机制。当我们请求的文件（css、js）很大时，可以使用 Cache-Control 实现缓存，从而达到性能优化的目的
**前提：使用相同的 URL 才能实现 Cache-Control 缓存机制**
```
HTML
<link rel = "stylesheet" href = "URL">
<script src = "URL">
```
#### 后端 + code
```
else if( path === '/js/main.js' ){
    response.setHeader( 'Cache-Control', 'max-age = 30' )  
    // 30s 内如果请求 main.js 文件，浏览器不发送请求，直接使用缓存中文件 ==> 下载时间 === 0
}
```

#### 特点
1. 让浏览器在一段时间内不访问服务器，不发送请求，直接使用本地硬盘 | 内存作为响应，从而减少请求时间
2. 首页（入口文件 + HTML）不设置 Cache-Control，因为在缓存的这段时间内，用户不能获取最新网页
3. 其他文件（css + js）会缓存很久（10年，甚至更久），如要更新，只需要改变入口文件（HTML）的 URL 即可，之后浏览器就会缓存最新版的文件
4. URL 改变实现方式：+ 查询参数 | + 随机数
![URL 改变实现 + 随机数](http://upload-images.jianshu.io/upload_images/9617841-69a7972df596f14d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### Expires
Expires 头指定了一个日期 | 时间， 在这个日期 | 时间之后，HTTP响应被认为是过时的

#### Cache-Control | Expires
从 Expires ==> Cache-Control 是 HTTP 升级的过程，以前使用 Expires 加缓存，现在使用 Cache-Control 加缓存，Expires 的问题在于，它的过期时间是本地的时间，如果本地时间错乱，可能导致用户一直不能使用缓存，从而影响用户体验
两者的区别在于：Cache-Control 设置缓存时长，Expires 设置缓存过期时间点。如果两者同时设置，Cache-Control 优先使用

# ETag
ETag HTTP 响应头是资源的特定版本的标识符。可以让缓存更加高效并节省宽带，如果内容没有改变，Web 服务器不需要发送完整的响应
#### MD5
MD5 指摘要算法，它可以把一个文件转化成一个字符串。若文件内容相同，则字符串相同。文件内容差异越小，字符串（算出来的结果）差异越大

#### 后端 + code
安装 MD5 ` npm install md5 `，然后 node.js 使用 MD5
```
else if( path === '/js/main.js' ){
    let string = fs.readFileSync( './js/main.js', 'utf-8' )
    let fileMd5 = md5( string )
    response.setHeader( 'ETag', fileMd5 )  // 响应头中有 ETag ==> ETag: md5 值
    // 当设置了 ETag 响应头，下次刷新时，请求中会多一个 If-None-Match 的请求头，值为 ETag 的值（md5 值）
    if( request.header[ 'if-none-match' ] === fileMd5 ){  // 如果请求的版本号（md5 值） === 浏览器的 If-None-Match 的值（md5 值） ==> 相同版本不需要下载
        // 没有响应体
        response.statusCode = 304  
        // 304 Not Modified 表示资源未被修改，因为请求头指定的版本If-Modified-Since或If-None-Match。在这种情况下，由于客户端仍然具有以前下载的副本，因此不需要重新传输资源。
    } else{
        response.statusCode = 200
        // 有响应体
        response.write( string )
    }
    response.end()
}
```

# 缓存机制
Cache-Control + ETag 联合使用

# 辨析
### Cookie + Session
1. Cookie 指某些服务器在浏览器终端的一些数据（通常经过加密），一般为了辨别用户身份，也可以储存少量信息
2. Session 是指服务器通过某种方式确定了用户身份后的会话状态，一般表现为服务器为每个用户单独存储的一部分数据
3. Session 是基于 Cookie 实现的，Cookie 是 Session 的基石
4. Cookie 存储在浏览器本地，用户可以看到内容。Session 存储在服务器，用户无法查看内容，一般 Session 的内容是进程\线程间共享的
5. Cookie 不安全，而 Session 解决了 Cookie 不安全的痛点

### Cookie + Storage
1. Cookie 和 Storage 都存储在本地的一个文件中
2. 两者都可以做跨页面通信，两者都不能跨域访问
3. Cookie 的每次请求**相同域名**时，都会带上 Cookie 里的所有内容去访问服务器
4. Storage 与 HTTP 无关，不会被带给服务器
5. Cookie 在做跨页面通信时，由于带上所有内容，导致上传数据 + 请求变慢，Storage 的出现解决了 Cookie 的痛点，只要将一些不敏感信息存储在 Storage 中即可
6. JS 调用 Cookie 比较麻烦，一般都用库进行封装。Storage 调用起来比较简单，也可以再次封装达到更好的效果
7. Cookie 大小 4K 左右，Storage 大小 5M 左右
8. 后台代码可以任意设置 Cookie 的过期时间。Storage 中的 LocalStorage 永久有效，除非用户删除，Storage 中的 SessionStorage 在用户关闭页面（Session 结束）后就失效

### LocalStorage + SessionStorage
1. 两者与 HTTP 无关
2. 每个域名的 LocalStorage | sessionStorage 有最大存储量，因浏览器而异
3. 只有相同域名的页面才能互相读取 LocalStorage。SessionStorage 只在同一浏览器窗口中共享
4. LocalStorage 本地存储， SessionStorage 会话存储
5. LocalStorage 永久有效，除非用户删除。SessionStorage 在用户关闭页面（Session 结束）后就失效

### Cache-Control + ETag
1. 两者都是 HTTP 响应头，都可以实现加快请求 | 响应速度
2. Cache-Control 是直接使用本地缓存，**不会发送请求**
3. ETag **发送请求**，如果 MD5 值相同，则没有响应体