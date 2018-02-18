---
title: Hello World
date: 2017-12-21 19:16:46
tags: 博客
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)


## 创建新博客并上传方法为：

1. ` hexo new 第二篇博客 `
2. start 第二篇博客.md 即编辑md文件
3. ` hexo generate `
4. ` hexo deploy `
5. end


## 开通博客方法

1. 创建一个目录，并使用 git bash 进入
2. 在 GitHub 上新建一个空 repository，repository 名称是「你的用户名.github.io」（请将你的用户名替换成真正的用户名）
3. npm install -g hexo-cli，安装 Hexo
4. hexo init myBlog
5. cd myBlog
6. npm i
7. hexo new 开博大吉
8. start 开博大吉.md，编辑这个 md 文件
9. start _config.yml，编辑网站配置
    把第 6 行的 title 改成你想要的名字
    把第 9 行的 author 改成你的大名
    把最后一行的 type 改成 type: git
    在最后一行后面新增一行，左边与 type 平齐，加上一行 'repo: 仓库地址' （请将仓库地址改为「你的用户名.github.io」对应的仓库地址）
    第 4 步的 repo: 后面有个空格，不要眼瞎。
10. npm install hexo-deployer-git --save，安装 git 部署插件
11. hexo deploy
12. 打开 GitHub Pages 功能
13. 点击预览链接

你现在应该看到了你的博客！

## 上传博客源代码

**注意：**你的用户名 .github.io 上保存的只是你的博客，并没有保存「生成博客的程序代码」，你需要再创建一个名为 blog-generator 的空仓库，用来保存 myBlog 里面的「生成博客的程序代码」。

1. 在 GitHub 创建 blog-generator 空仓库

2. 使用 SSH 创建

这样一来，你的博客发布在了「你的用户名.github.io」而你的「生成博客的程序代码」发布在了 blog-generator。所有数据万无一失，你就不会因为误删 myBlog 目录而痛哭了

以后每次 hexo deploy 完之后，博客就会更新；

然后你还要要 add / commit / pull / push  一下「生成博客的程序代码」，以防万一

这个 blog-generator 就是用来生成博客的程序，而「你的用户名.github.io」仓库就是你的博客页面

## 更改博客主题

1. [主题合集](https://github.com/hexojs/hexo/wiki/Themes)
2. 找一个喜欢的主题，进入主题的 GitHub 首页，复制它的 SSH 地址或 HTTPS 地址
3. git clone '地址'
4. ls  复制下载的主题的名称
5. cd ..
6. 编辑 _config.yml 文件，第 75 行改为 theme: '主题名称'，保存
7. hexo generate
8. hexo deploy
9. end