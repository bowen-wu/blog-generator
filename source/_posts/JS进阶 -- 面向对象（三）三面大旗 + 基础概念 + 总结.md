---
title: JS进阶 -- 面向对象（三）三面大旗 + 基础概念 + 总结
date: 2018-02-26 07:38:51
tags: JavaScript 进阶
---
## 概述
面向对象是一种思维定势（套路），当遇到任何需求，都是用这一套思维解决。本篇我们进一步了解面向对象，主要了解面向对象的三面大旗，即封装 + 继承 + 多态，还有一些关于面向对象的概念

## 三面大旗
#### 封装
封装即是**隐藏细节**，可以是 A 对 A 隐藏细节，也可以是 A 对 B 隐藏细节。任何一个函数都是封装
- A - A ==> 封装函数，解决自己的思想负担，需要使用时只需要调用自己封装的函数即可
- A - B ==> 例如一些第三方库（jQuery 的作者对于使用 jQuery 的人），A 给 B 提供 API，B 使用 API，有助于合作 + 交流

面向对象可以实现封装，但不代表只有面向对象可以封装

#### [继承](https://www.jianshu.com/p/4a5170bc9afd)

#### 多态
多态使得一个东西**灵活**，如果没有多态，则经常要进行类型转换，指一个东西拥有两种甚至更多种属性
示例：div 同时是 Node + Element，可以调用两者任意的 API
```
div.childNodes  // 子代节点 ==> div 作为 Node 使用
div.child    // 元素节点 ==> div 作为 Element 使用
```
JavaScript 是动态语言，所有东西都是多态，所有东西都有多个状态

## 基础概念
**面向对象**是把一组数据结构和处理它们的方法组成**对象（Object）**，把相同行为的对象归纳为**类（Class）**，通过类的**封装（Encapsulation）**隐藏内部细节，通过**继承（Inheritance）**实现类的**特化（Specialization）**/**泛化（Generalization）**，通过**多态（Polymorphism）**实现基于对象类型的**动态分派（Dunamic Dispatch）**
面向对象是一套经验（套路）。它可以使得我们更快、更好的进行开发

#### Namespace 命名空间
它是一种可以捆绑功能的容器，经常是一个对象，可以为这个对象添加属性和方法，这个对象就是命名空间，例如 window.jQuery

#### Class 类
定义对象的特征，它是对象属性和方法的模板定义

#### Object 对象
类的一个实例

#### Property 属性
对象的特征，比如大小

#### Method 方法
对象的能力，比如行走

#### Constructor 构造函数
对象初始化的瞬间，被调用的方法，通常它的名字和包含它的类一致

#### Inheritance 继承
一个类可以继承另一个类的特征

#### Encapsulation 封装
把数据和相关的方法绑定在一起使用的方法

#### Abstraction 抽象
结合复杂的继承、方法和属性的对象可以模拟现实的模型

#### Polymorphism 多态
不同的类可以定义为相同的方法或属性

## 总结
#### JavaScript 实现面向对象
JavaScript 中的面向对象都是对于复杂类型（Object）而言的，JavaScript 中 Object 又主要分为 ` 普通对象 + Array + Function `，JavaScript 中没有类的概念，所以 JavaScript 中使用原型（` __proto__ `，原型链）实现继承

#### 代码层面的面向对象
```
function Person( name ){
    this.name = name
}
Person.prototype.say = function(){
    console.log( ` My name is ${ this.name } ` )
}
var xiaoming = new Person( 'xiaoming' )
xiaoming.say()
```
这就是代码层面的面向对象，` function Person(){} ` 就是调用的函数， ` Person.prototype.say = function(){} ` 就是你生成的新对象的原型方法


#### 面向对象解决的问题
1. 组件化。使得代码划分明确，作用清晰
2. 调用时只需要 ` new `，即可把公共属性 + 私有属性继承
3. 每一个 ` new ` 之间互不影响，之后便可以添加自己的属性（也可覆盖公共属性）


## 相关知识点
#### 命名空间
```
var app = app || {}
```
我们先来了解下 ` && ` 和 ` || ` 这两个操作符的含义。这两个操作符返回的是一个 truey | falsey 值

###### && 操作符
![&& 操作符](http://upload-images.jianshu.io/upload_images/9617841-ec8819c975b440dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### || 操作符
![|| 操作符](http://upload-images.jianshu.io/upload_images/9617841-2b30afb11d2718ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以当我们使用 ` var app = app || {} ` 这种写法是最佳实践，因为如果在这个语句之前，` app ` 就有了定义，那么这个语句不会覆盖之前的定义。