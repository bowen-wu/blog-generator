---
title: HTTP系列 -- 基础知识
date: 2018-01-15 18:21:51
tags: HTTP
---
[代码链接](https://github.com/bowen-wu/bash-create-file-script-demo)
# 脚本
JavaScript 是一门动态类型、面向对象的脚本语言

### bash 脚本
```
mkdir ~/local
cd ~/local
touch demo.txt
start demo.txt

    //内容如下：
    //mkdir demo
    //cd demo
    //mkdir css js
    //touch index.html css/style.css js/main.js
    //exit

    //（Windows 用户请跳过这一步）给 demo.sh 添加执行权限，加上可执行的操作
    //chmod +x demo.txt

    // 在任意位置执行 'sh ~/local/demo.txt' 即可运行此脚本，sh ==> shell
cd ~/desktop
sh ~/local/demo.txt
    // 你会看到当前目录里多出一个 demo 目录，demo 目录里面还有一些文件
    // demo.txt 就是你写出的第一个 Bash 脚本了。

    // 将 ~/local 添加到 PATH 里
cd ~/local;pwd
touch ~/.bashrc
start ~/.bashrc
    //在最后一行添加 
export PATH="local的绝对路径:$PATH"
source ~/.bashrc
    // 之前你要运行 sh ~/local/demo.txt，现在你只需要运行 demo.txt 就行  了
    // 原因：将这个路径添加到 '~/.bashrc' 之后，就是在进入 git bash 之前就运行了 '~/.bashrc' 

    // 删掉demo.txt 的后缀 .txt 
mv ~/local/demo.txt ~/local/demo
    // 现在只要运行 demo 就能执行该脚本了。
```

##### 说明：
1. PATH 的作用
你每次在 Bash 里面输入一个命令时（比如 ls、cp、demo），Bash 都会去 PATH 列表里面寻找对应的文  件，如果找到了就执行。(`each $PATH`查看PATH，'目录：目录...')
2. `type demo` 可以看到寻找过程
3. `which demo` 可以看到寻找结果,最终结果
4. 文件后缀的作用：毫无作用，windows有一个用处，告诉计算机，用什么打开文件
5. 所有命令都是可执行文件，都是一个脚本文件，可执行文件就是命令，不可执行文件就是配置

##### 参数
让 demo 脚本创建的目录可变，更改 demo 的内容为
```
mkdir $1
cd $1   //  $1 ==> 表示传递的第一个参数
mkdir css js
touch index.html css/style js/main.js
exit
```
   
##### 返回值
`exit 1` 表示错误代码1

`exit 0`表示没有错误
```
demo xxx && echo 'end' //表示之后 demo xxx 成功了才会执行 echo 'end'
```

### [Node.js 脚本](https://github.com/bowen-wu/node-create-file)

# www（World Wide Web）
1990年万维网（World Wide Web）诞生。Tim Berners-Lee 发明了第一个网页、第一个浏览器和第一个服务器

### 主要概念
1. **URI（Uniform Resource Identifier）**：统一资源标识符，是一个用于标识某一互联网资源名称的字符串，其中包括 URL 和 URN 。
    - URL（Uniform Resource Locator）：统一资源定位符，或者称为**URL地址**和**网页地址（网址）**。统一资源定位符的标准格式如下：
    ` 协议类型:[//服务器地址[:端口号]][/资源层级UNIX文件路径]文件名[?查询][#片段ID] `
    ![URL地址](http://upload-images.jianshu.io/upload_images/9617841-1dd60cd247108428.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    - URN（Uniform Resource Name）：统一资源名称，其目的是通过提供一种途径，用于在特定的命名空间资源的标识，以补充网址。

2. **HTTP（HyperText Transfer Protocol）**：超文本传输协议，是一种用于分布式、协作式和超媒体信息系统的应用层协议。就是两个电脑之间传输内容的协议。

3. **HTML（HyperText Markup Language）**：超文本标记语言，是一种用于创建网页的标准标记语言。[第一个网页](http://info.cern.ch/)

4. **DNS（Domain Name System）**：域名系统，是互联网的一项服务。它作为将**域名和IP地址相互映射**的一个分布式数据库，能够使人更方便地访问互联网。（**输入一个域名，将得到一个IP**）
    - 输入域名
    ```
    //在命令行中输入以下任意一条，即可返回包括IP地址的一些信息
    nslookup baidu.com
    ping baidu.com
    ```
    - 输出IP

**说明：**
- 可以修改本地 hosts 文件从而让域名指向特定的IP
- URL 的作用是能让你访问一个页面
- HTTP 的作用是让你能下载这个页面
- HTML 的作用是让你能看懂这个页面。