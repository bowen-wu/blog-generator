---
title: JS进阶 -- 面向对象（一）代码 + 函数 + this + new
date: 2018-02-22 07:41:17
tags: JavaScript 进阶
---
## 概述
本篇我们借助 MVC 来初步理解面向对象，之后理解一下 函数 + ` this ` + ` new `

## 代码
#### 初始
当我们使用 MVC 思想将代码模块化后，每个 js 文件中代码没有重复，很精简，但是当我们跨文件观察我们的 js 时，发现每个 js 文件都有
```
let view = document.querySelect( 'xxx' )
let model = {
    init: function(){},
    fetch: function(){},
    save: function(){}
}
let controller = {
    view: null,
    model: null,
    init: function(){
        this.view = view
        this.model = model
        this.model.init()
        this.init()
        this.bingEvents()
    }
}
```
这是一种跨文件的重复，如果我们抽丝剥茧便可以对重复的地方进行分类，在全局范围内创造一个模板，每一个重复的地方都是由全局**模板**制造出来的

#### 进阶
我们创建一个 base 文件目录，里面有
```
base/View.js
    window.View = function(){}
base/Model.js
    window.Model = function(){}
base/Controller.js
    window.Controller = function(){}
```
之后每个文件的重复的地方都使用全局变量 View()、Model() 和 Controller()

## 函数 + this
函数是一种可执行代码组成的对象，` this ` 仅仅是函数的一个**参数**而已
#### 特点
I/O + 一顿操作 + 隐藏细节，所有的 I( input ) === ` this + arguments `

#### 函数 <==> this <==> 对象
JavaScript 通过 ` this ` 连接 ` Object ` 和 ` Function `，函数和对象没有一丢丢关系
```
let obj = {
    name: 'xxx',
    sayName: function() {
        console.log( this.name )
    }
}
```
obj.sayName 仅仅存储了函数的地址而已 ==> 函数是一等公民 ==> 函数是一个独立的，既不是附属品，也不是方法

#### [判断 this 的值](https://www.jianshu.com/p/66a32662e920)

## new 操作符
#### 概述
new 操作符主要是一个语法，它主要做了
```
function Person(name) {
    this.name = name
}

当使用 let xiaoming = new Person( 'xiaoming' ) 时，等价于

let xiaoming = function(options) {
    // 1. var temp = {}
    // 2. this = temp
    this.name = 'xiaoming'
    // 3. 构造函数.prototype = { constructor: 构造函数 }
    // 4. this.__proto__ = 构造函数.prototype
    // 5. return this
}
```
**请注意：new 返回值的类型是 Object**
![浏览器运行结果](http://upload-images.jianshu.io/upload_images/9617841-75082f65d6acfe94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**说明**：如果人为在 Person 中增加 ` return `，两种情况
- ` return 基本类型 ` ==> 无效 + 无视
- ` return 复杂类型（对象） ` ==> 返回复杂类型（对象）

#### new 的实现
**` var 对象 = new 函数 `**
##### 方法一：通过 Object.create() 实现 new
```
function _new(){
    let constructor = arguments[0]
    let obj = Object.create( constructor.prototype )
    constructor.apply( obj, [].slice.call( auguments, 1 ))
    return obj
}
```
![通过 Object.create() 实现 new](http://upload-images.jianshu.io/upload_images/9617841-ad27b22da142fdae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 方法二：通过 Object.setPrototypeOf() 实现 new
```
function _new(){
    let constructor = arguments[0]
    let obj = {}
    Object.setPrototypeOf( obj, constructor.prototype )
    constructor.apply( obj, [].slice.call( arguments, 1 ))
    return obj
}
```
![通过 Object.setPrototypeOf() 实现 new](http://upload-images.jianshu.io/upload_images/9617841-b73dae67e0ce13ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 相关知识点
创建一个函数过程
1. 创建一个函数对象

2. 创建一个原型对象

3. 函数对象会有一个 ` prototype ` 属性，指向对应的原型对象

4. 原型对象中有一个 ` constructer ` 属性，指向对应的构造函数

![函数](http://upload-images.jianshu.io/upload_images/9617841-0d655d27ee7edf46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)