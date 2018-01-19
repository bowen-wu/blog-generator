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
1. 浏览器检查域名，如果缺少端口号，加上默认的端口号 `80` ，如果缺少路径，加上默认路径 `/` ，如果缺少协议，加上默认协议 `http` 
2. 浏览器将请求的域名发送给路由器。
3. 路由器拿到请求的域名后询问服务商此域名所对应的 IP 地址（服务商的 **DNS** 服务）。
4. 路由器拿到相应的 IP 地址，发起 HTTP 请求（**Request Headers**）。
5. 相应 IP 地址的服务器将在 80 端口接收路由器的请求。
6. 服务器查看路径，`setHeader` ，之后返回 `HTML` 。
7. 服务器响应并返回内容给路由器。
8. 路由器接收响应（内容）。
9. 路由器将响应（内容）传递给浏览器。
10. 浏览器下载相应的 `HTML` 。
11. 浏览器解析，并查看是否还有要请求的东西，如果有，再次发起 `HTTP` 请求。
12. 所有的东西都请求结束，浏览器渲染到页面。

**注意**：后缀是没有用的。HTTP 路径不是文件路径。

