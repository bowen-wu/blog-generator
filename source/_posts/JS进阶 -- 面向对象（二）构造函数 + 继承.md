---
title: JS进阶 -- 面向对象（二）构造函数 + 继承
date: 2018-02-23 08:13:53
tags: JavaScript 进阶
---
## 概述
本篇主要讲述构造函数和继承

## 构造函数
#### 编程
编程主要分为**函数式编程**和**面向对象编程**

-  函数式编程 ==> 推崇函数

- 面向对象编程 ==> 推崇**类**，没有函数概念
#### JavaScript
JavaScript 中的**类**可以理解为**构造函数**

- 类 ==> 如果一个东西返回一个 ` Object ` 就叫做类

- 构造函数 ==> 如果一个函数返回一个 ` Object ` 就叫做**构造函数**

因为 JavaScript 中没有类的概念，所以 JavaScript 中的构造函数即可以看做是类，JavaScript 中 ` new ` 是构造函数的典范

## 继承
继承可以实现**代码复用**，JavaScript 中使用原型（` __proto__ `，原型链）实现继承，实例化对象的 ` __proto__ ` 指向构造函数的 ` prototype `
```
对象.__proto__ === 函数.prototype   // 很重要
```
### 分类
1. 对象之间的继承 ==> ` __proto__ `

2. 函数与对象之间的继承通过 ` this ` 参数（关键字）传导

3. 关键字 ` new ` 形成构造函数，从而使用函数创建对象，使得新创建的对象可以继承构造函数上的属性



### 模拟继承的具体方式
- 共有属性 ==> ` prototype ` 属性
- 私有属性 ==> 构造函数上的属性

==> 继承 === 共有属性 + 私有属性 === ` prototype ` 属性 + 构造函数上的属性

```
function Person( name ){
    this.name = name
}
Person.prototype.think = true
Person.prototype.foot = 'two'
Person.prototype.say = function(){
    console.log( 'I can say' )
}
Person.prototype.walk = function(){
    console.log( 'I can walk' )
}
```

##### new 模拟继承
```
let xiaoming = new Person( 'xiaoming' )
xiaoming.a = 1
xiaoming.xxx = function(){
    console.log( 'xxx' )
}
```
对象 ` xiaoming ` 具有 ` Person ` 的私有属性 + 共有属性，还有自身的属性
![new 模拟继承](http://upload-images.jianshu.io/upload_images/9617841-61fa30713fb1959e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##### call + Object.create() + new 模拟继承
new 模拟的继承仅仅有一层，如果我们要模拟多层，即 ` xiaoming ` 继承 ` 
Student `，` Student ` 继承 ` Person `
![示例图](http://upload-images.jianshu.io/upload_images/9617841-195820e44e5b350f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
其中 ` Student ` 继承 ` Person ` 是**构造函数与构造函数的继承（类与类的继承）**

```
function Student( name, school ){
    Person.call( this, name )   // 继承 Person 属性  === Person.bind( this ).( name )
    this.school = school
}
Student.prototype = Object.create( Person.prototype )   // 继承 Person 方法
Student.prototype.learn = function(){
    console.log( `${ this.name } learn in ${ this.school }` )
}
let xiaoming = new Student( 'xiaoming', 'qinghua' )
xiaoming.a = 1
xiaoming.xxx = function(){
    console.log( 'xxx' )
}
```
对象 ` xiaoming ` 既继承了 ` Student ` 的共有属性 + 私有属性，又继承了 ` Person ` 的共有属性 + 私有属性，还有自己的属性
![call + Object.create() + new 实现继承](http://upload-images.jianshu.io/upload_images/9617841-1caaa8c9aa72cd21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 模拟构造函数与构造函数的继承（类与类的继承）
上面中模拟构造函数与构造函数的继承（类与类的继承）使用了 ` Object.create() `。模拟构造函数与构造函数的继承（类与类的继承）即是实现
```
Student.prototype.__proto__ = Person.prototype
```
- 方案1：前提 ==> ` Person ` 为空（没有私有属性），若 ` Person ` 不为空，则 ` Student ` 上会多出 ` Person ` 上的私有属性
    ```
    Student.prototype = new Person()
    ==> Student.prototype === this
    ==> Student.prototype.__proto__ === Person.prototype
    ```
- 方案2：ES3 ==> not care Person 上的私有属性
    ```
    function emptyPerson() {}
    emptyPerson.prototype = Person.prototype
    Student.prototype = new emptyPerson()
    ```

- 方案3：ES5 API
    ```
    Student.prototype = Object.create( Proson.prototype )
    ```

- 方案4：ES6 ==> class + extends ==> prototype(共有属性)必须是函数
    1. ` class ` 用法：
        ```
        class Example{
            constructor(options){
                this.name = options.name
            }
            say() {  }
            walk() {  }
        }
        == 等价于 ==
        function Example(options) {
            this.name = options.name
        }
        Example.prototype.say = function() {  }
        Example.prototype.walk = function() {  }
        ```

    2. extends 用法：
        ```
        class Student extends Person === Student.prototype = Object.create(Person.prototype)
        super(options) === Example.call(this,options)
        ``` 
    3. 具体实现
        ```
        class Student extends Person{
            constructor(options){
                super(options)   // 继承私有属性
            }
        }
        ```
    4. ` class + extends ` 不伦不类
        ` Student ` 是 ` class `（类） ==> JavaScript 中没有类这个数据类型
        ` Student ` 是函数 ==> Student 并不能 ` call `

## 相关知识点
` Object.create() ` 的实现
```
function inherit( base ){
    function fn(){}
    fn.prototype = base
    return new fn()
}
```