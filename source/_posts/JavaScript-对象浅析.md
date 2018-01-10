---
title: JavaScript 对象浅析
date: 2018-01-10 12:52:55
tags: JavaScript
---
全局对象 global(特例：window)

# window 的重要属性：

### 分类：

1. ECMAScript 规定的

    - paeseInt()

    - parseFloat()

    基本包装类型：Boolean()  Number()  String()

    - Number()

    - String()

    - Boolean()

    - Object()

        **声明对象：**
        1. var obj1 = {} 
        2. var obj2 = new Object(); //obj1 和 obj2 没有区别，但是 obj1 === obj2 是 false ，因为存储在 Stsck(栈内存) 中的地址不同。
        3. var obj3 = Object.create(Object.prototype)

    - setTimeout()

2. 浏览器私有的(每个浏览器是不一定相同的)

    - alert() 

    - prompt()

    - comfrim()

    - console.log()

    - document ==> 文档，表示了可以对文档所做的一切操作

    - history ==> 
# 基本包装类型 和 Object()

### 基本包装类型

基本包装类型有属性 `__proto__` ，这个属性引用（指向）了在 Heap( 堆内存 ) 的一个地址，这个地址表示**基本包装类型对应的 prototype（原型）**。

![基本包装类型 Boolean](http://upload-images.jianshu.io/upload_images/9617841-4911258278ef065e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![基本包装类型 Number](http://upload-images.jianshu.io/upload_images/9617841-6e664f59886709f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![基本包装类型 String](http://upload-images.jianshu.io/upload_images/9617841-5c889bdc59874e0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

基本包装类型有**重写属性**（表示重写属性从而覆盖 **Object 的 prototype**）。

### Object()

每个基本包装类型的 `__proto__` 属性和 Object() 都有一个属性 `__proto__` ，这个属性引用（指向）了在 Heap( 堆内存 ) 的一个地址，这个地址表示 **Object() 的 prototype**。

### 原型链

### Object.prototype（原型）

var 对象 = new 函数
##### 对象.__proto__  ===  函数.prototype
__proto__ 是对象的属性
prototype 是函数的属性

"1".__proto__ === String.prototype
1.__proto__ === Number.prototype
true.__proto__ === Boolean.prototype

一些属性和方法都挂在到了window上，当没有一行代码的时候，也会有window对象。
