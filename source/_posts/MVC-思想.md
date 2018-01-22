---
title: MVC 思想
date: 2018-01-22 22:13:10
tags: JavaScript 高级
---
# 概述
MVC 是一种思想，它会让我们写代码时更加简洁、高效、模块化，了解这种方法对于前端程序员来说是很有必要的。首先说明下 MVC 代表的含义：
- M ==> Model ==> 模型
- V ==> View ==> 视图
- C ==> Controller ==> 控制器

# MVC 各自作用

MVC 主要是按功能划分模块

1. view：js 代码所操纵的视图（HTML），负责显示给用户和通知 controller（控制器）
2. Model：view（视图）所需要的所有的数据操作,包括：
    - 初始化数据
    - 读取数据
    - 存入数据

    负责和向 server（服务器）请求数据和响应 server（服务器） 

3. controller：所有的逻辑操作，负责监控和更新 view（视图） + 调用和接收 model（数据） 

# MVC 使用技术点

1. 立即执行函数，[这里](https://www.jianshu.com/p/e8ad9fdf8e07)有所有的立即执行函数的写法，但是这里推荐大家使用 
    ``` 
    !function(){}.call() 
    ```
    立即执行函数避免了我们是用全局变量，隔离作用域，从而达到了使用局部变量的目的

2. 闭包
    闭包就是如果一个函数使用了它范围外的值，那么这个函数 + 这个变量就叫做闭包
    闭包使得模块之间可以进行相互访问。具体实现：
    ![闭包 + 操作局部变量](http://upload-images.jianshu.io/upload_images/9617841-10ee3cec1d8d6750.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    - 函数 fn 中局部变量 obj 和 return 的函数构成了闭包
    - return 的函数便是接口，可以供外部调用，从而来操纵局部变量 obj
    - **外部除了使用函数 fn 的接口，没有任何其他方法可以访问到局部变量 obj**

3.  全局变量
模块与模块之间是在不同的 js 文件中，例如模块2想去操纵模块1的数据，通过上述方式显然是不够的，那么只能退而求其次，使用全局变量。具体实现：
    ![全局变量](http://upload-images.jianshu.io/upload_images/9617841-9012bcc7bdc3e5fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
通过使用全局变量保存了匿名函数的地址，从而全局都可以使用这个全局属性

### 总结
以上3种技术点的结合 ==> **立即执行函数 + 闭包 + 全局变量** 实现了 MVC 中 Controller（控制器） 的相应需求，从而让前端更好的使用 MVC 思想

#### 细节问题
1. this 的使用，在 MVC 中要着重使用 this 
2. 箭头函数，其实在 MVC 中使用箭头函数，主要就是为了 this ，原因在于**箭头函数内外 this 不变**
3. HTML 中每一块都是一个 view ，当我们为 HTML 分块的时候，是可以嵌套的，所以我们的 view 也是可以嵌套的

# 使用模板
```
!function(){
    let view = 操纵的 HTML 模块
    let model = {
        init: function(){},
        fetch: function(){}  // 注意此处最好返回 Promise 对象
        save: function(){}  // 注意此处最好返回 Promise 对象
    }
    let controller = {
        view = null,
        model = null,
        ...... = null,
        init: function(){
            this.view = view
            this.model = model
            ......
            this.bindEvents()
        },
        bindEvents: function(){
            // 绑定事件，与绑定事件无关的放在 controller 属性上
        },
        ......
    }
    controller.init( controller,model )
    //controller.init.call( controller,view, model )
}.call()
```

# 实际应用
[代码示例](https://github.com/bowen-wu/resume/blob/master/js/leaveMessage-system-database.js)
