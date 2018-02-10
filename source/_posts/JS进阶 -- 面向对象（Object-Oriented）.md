---
title: JS进阶 -- 面向对象（Object-Oriented）
date: 2018-02-08 07:35:39
tags: JavaScript 进阶
---
# 概述
面向对象是一种前端前辈们总结出的一套经验，因为它是一种经验，所以每个前端都对其有自己的理解，但毫不保留的说它可以使我们更快、更好的进行开发。它的中心思想可以理解为 **你办事，我放心**

# 概念
**面向对象**是把一组数据结构和处理它们的方法组成**对象（Object）**，把相同行为的对象归纳为**类（Class）**，通过类的**封装（Encapsulation）**隐藏内部细节，通过**继承（Inheritance）**实现类的**特化（Specialization）**/**泛化（Generalization）**，通过**多态（Polymorphism）**实现基于对象类型的**动态分派（Dunamic Dispatch）**

###### 注意：现在看不懂没有关系，先往下看。


# 技术术语
### Namespace 命名空间
它是一种可以捆绑功能的容器，经常是一个对象，可以为这个对象添加属性和方法，这个对象就是命名空间，例如 window.jQuery

### Class 类
定义对象的特征，它是对象属性和方法的模板定义

### Object 对象
类的一个实例

### Property 属性
对象的特征，比如大小

### Method 方法
对象的能力，比如行走

### Constructor 构造函数
对象初始化的瞬间，被调用的方法，通常它的名字和包含它的类一致

### Inheritance 继承
一个类可以继承另一个类的特征

### Encapsulation 封装
把数据和相关的方法绑定在一起使用的方法

### Abstraction 抽象
结合复杂的继承、方法和属性的对象可以模拟现实的模型

### Polymorphism 多态
不同的类可以定义为相同的方法或属性

# 示例
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
这是一种跨文件的重复，如果我们抽丝剥茧便可以对重复的地方进行分类，在全局范围内创造一个模板，每一个重复的地方都是有全局模板制造出来的，这个模板就是之前提到的 **类**。

我们创建一个 base 文件目录，里面有
```
base/View.js
    window.View = function(){}
base/Model.js
    window.Model = function(){}
base/Controller.js
    window.Controller = function(){}
```
之后每个文件的重复的地方都使用全局变量 View()、Model() 和 Controller() ，这三个全局变量便是之前提到的 **构造函数**

# new 操作符
new 操作符主要是一个语法，它主要做了：
1. ` var temp = {} `
2. ` this = temp `
3. ` 构造函数.prototype = { constructor: 构造函数 } `
4. ` this.__proto__ = 构造函数.prototype `
5. ` return this `
```
构造函数.prototype = {
    constructor: 构造函数,
    // 共有属性
}
```
### 制作一个简单的 new
```
function Human(options){
    this.name = options.name
    this.city = options.city
}
Human.prototype.constroctor = Human
Human.prototype.species = 'person'
Human.prototype.walk = function(){
    console.log( ' I can walk ' )
}
Human.prototype.useTools = function(){
    console.log( ' I can use tool ' )
}
var human = new Human({name:'xxx',city:'hangzhou'})
```
![new](http://upload-images.jianshu.io/upload_images/9617841-d04e85a5e0576d2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 最佳实践
### 命名空间
```
var app = app || {}
```
我们先来了解下 && 和 || 这两个操作符的含义。这两个操作符返回的是一个 truey | falsey 值

##### && 操作符
![&& 操作符](http://upload-images.jianshu.io/upload_images/9617841-ec8819c975b440dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### || 操作符
![|| 操作符](http://upload-images.jianshu.io/upload_images/9617841-2b30afb11d2718ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以当我们使用 ` var app = app || {} ` 这种写法是最佳实践，因为如果在这个语句之前，` app ` 就有了定义，那么这个语句不会覆盖之前的定义。

###### 注意： 现在再去回头看一眼那个概念，相信你会有深入的理解