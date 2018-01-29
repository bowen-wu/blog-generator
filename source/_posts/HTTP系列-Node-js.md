---
title: HTTP系列 -- Node.js
date: 2018-01-19 08:01:38
tags: HTTP
---
[Node.js 服务器代码](https://github.com/bowen-wu/Node.js-server-JSONP-AJAX/blob/master/server.js)
# 精华
```
let string = fs.readFileSync( '路径', 'UTF-8' )
response.setHeader( 'Content-Type', '类型' )
response.statusCode = 200
response.write()
response.end()
```
### response.setHeader( 'Content-Type', '类型' )
类型：
- HTML ==> response.setHeader( 'Content-Type', 'text/html; charset = utf-8' )
- CSS ==> response.setHeader( 'Content-Type', 'text/css' )
- JavaScript ==> response.setHeader( 'Content-Type', 'application/javascript' )

# 创建 Node.js 服务器
```
var http = require('http')
var fs = require('fs')
var url = require('url')
var port = process.argv[2]

if(!port){
  console.log('请指定端口号好不啦？\nnode server.js 8888 这样不会吗？')
  process.exit(1)
}

var server = http.createServer(function(request, response){
  var parsedUrl = url.parse(request.url, true)
  var path = request.url 
  var query = ''
  if(path.indexOf('?') >= 0){ query = path.substring(path.indexOf('?')) }
  var pathNoQuery = parsedUrl.pathname
  var queryObject = parsedUrl.query
  var method = request.method

  /******** 从这里开始看，上面不要看 ************/


  console.log('HTTP 路径为\n' + path)
  if(path == '/style'){
    response.setHeader('Content-Type', 'text/css; charset=utf-8')
    response.write('body{background-color: #ddd;}h1{color: red;}')
    response.end()
  }else if(path == '/script'){
    response.setHeader('Content-Type', 'text/javascript; charset=utf-8')
    response.write('alert("这是JS执行的")')
    response.end()
  }else if(path == '/index'){
    response.setHeader('Content-Type', 'text/html; charset=utf-8')
    response.write('<!DOCTYPE>\n<html>'  + 
      '<head><link rel="stylesheet" href="/style">' +
      '</head><body>'  +
      '<h1>你好</h1>' +
      '<script src="/script"></script>' +
      '</body></html>')
    response.end()
  }else{
    response.statusCode = 404
    response.end()
  }


  /******** 代码结束，下面不要看 ************/
})

server.listen(port)
console.log('监听 ' + port + ' 成功\n请用在空中转体720度然后用电饭煲打开 http://localhost:' + port)
```

# 解析 Node.js 服务器
1. ` console.log('HTTP 路径为\n' + path) ` 表示在命令行中或者`Console`面板中打印出当前请求的 HTTP 路径

2. 下方代码表示如果请求路径为` /style `，设置响应头信息为`'Content-Type', 'text/css; charset=utf-8'`（即响应内容 [ 第四部分 ] 的类型 [ css ] 和编码 [ utf-8 ]），响应内容( 第四部分 )为 ` body{background-color: #ddd;}h1{color: red;} ` ，之后完成响应
    ```
    if(path == '/style'){
        response.setHeader('Content-Type', 'text/css; charset=utf-8')
        response.write('body{background-color: #ddd;}h1{color: red;}')
        response.end()
      }
    ```

3. 下方代码表示如果请求路径为 ` /script ` ，设置响应头信息为 ` 'Content-Type', 'text/javascript; charset=utf-8' ` （即响应内容 [ 第四部分 ] 的类型 [ javascript ] 和编码 [ utf-8 ]），响应内容( 第四部分 )为 ` alert("这是JS执行的") `，之后完成响应
    ```
    if(path == '/script'){
        response.setHeader('Content-Type', 'text/javascript; charset=utf-8')
        response.write('alert("这是JS执行的")')
        response.end()
      }
    ```
4. 下方代码表示如果请求路径为 ` /index ` ，设置响应头信息为 ` ('Content-Type', 'text/html; charset=utf-8' ` （即响应内容 [ 第四部分 ] 的类型 [ html ] 和编码 [ utf-8 ]），响应内容( 第四部分 )为 HTML 页面，之后完成响应
    ```
    if(path == '/index'){
        response.setHeader('Content-Type', 'text/html; charset=utf-8')
        response.write('<!DOCTYPE>\n<html>'  + 
          '<head><link rel="stylesheet" href="/style">' +
          '</head><body>'  +
          '<h1>你好</h1>' +
          '<script src="/script"></script>' +
          '</body></html>')
        response.end()
      }
    ```

5. 下方代码表示如果请求的路径不是上述路径，则返回状态码为 `404` ，之后完成响应。
    ```
    else{
        response.statusCode = 404
        response.end()
      }
    ```

# 创建过程
1. 新建目录和进入
    ```
    mkdir node-demo
    cd node-demo
    ```

2. 创建文件并编辑
    ```
    touch server.js
    start server.js //内容如上
    ```

3. 运行文件
    ```
    node server.js 端口号
    ```

#使用 Node.js 发起请求
1. 在新的 Bash 窗口运行 `curl http://localhost:你的指定的端口/xxx` 或者 `curl http://127.0.0.1:你指定的端口/xxx`即可发起请求

2. 在浏览器窗口地址栏中输入 `http://localhost:你的指定的端口/xxx` 或者 `http://127.0.0.1:你指定的端口/xxx` 回车，即可发起请求。

3. `curl -s -v -- "http://localhost:port/xxx"`可以查看完整的请求体和响应体。

# 响应
- 在运行 `node server.js 端口号` Bash 窗口中可以查看到打印出来的路径（`console.log('HTTP 路径为\n' + path)`），并且在发起请求的 Bash 窗口中可以查看请求体和响应体

- 在浏览器窗口中可以看到实际运行效果

# 相关知识点
1. 任意一台电脑都是服务器

2. 创建的*Node.js服务器*提供 HTTP 服务，所以需要提供 HTTP 服务的程序

3. 用脚本可以提供 HTTP 服务


# 当在浏览器地址栏输入地址回车后发生的事情
1. DNS 域名解析。首先浏览器会查找缓存，如果没有，就向运营商查找，运营商会提供 DNS 服务，从而得到域名所对应的 IP 地址。DNS 域名解析就是一个域名到 IP 地址的转换

2. TCP 连接。浏览器根据 IP 地址向服务器发起 TCP 连接，与服务器建立 TCP 三次握手，浏览器对服务器说：“您好，我可以链接你么？”，服务器对浏览器说：“您好，你可以连接我”，浏览器对服务器说：“好的，那我连接你了！”，通过 TCP 的三次握手，从而确定双方都有收发文件的能力，自此浏览器和服务器建立了链接

3. 浏览器发送 HTTP 请求。请求包括了请求行、请求头和请求正文


5. 浏览器拿到响应、解析并渲染页面。浏览器是一个边解析边渲染的过程，首先浏览器解析HTML文件构建 DOM 树，然后解析 CSS 文件构建渲染树，等到渲染树构建完成后，浏览器开始布局渲染树并将其绘制到屏幕上

6. 通过四次挥手关闭 TCP 连接。浏览器对服务器说：“不早了，我该走了”，服务器对浏览器说：“知道了”，之后服务器又对浏览器说：“我也该走了”，浏览器回应服务器：“好的”，至此 TCP 连接结束

**注意**：后缀是没有用的。HTTP 路径不是文件路径。

