---
title: 总结篇（三） -- 你知道 log 出来的 this 是什么么
date: 2018-02-20 08:39:41
tags: 总结
---
## 概述
函数中的 ` this ` 是一个很重要的知识点，如果能清楚的知道各种场景下函数中 ` this ` 所代表的值，那么对于我们去理解库 | 框架是很有帮助的，也可以让我们去运用一些更加高级的思想

## this
` this ` 的使用情况大致分为两种
- 函数中
- 对象的方法中
```
this === call | apply | bind 的第一个参数
```
在判断 ` this ` 的值的时候，进行相应的转化是很有必要的

#### 严格模式 + 非严格模式
## 示例一
```
'use strict'
function fn( a, b ){
    console.log( this )
}
fn( 1, 2 )
```
上述中的 ` this ` 会打印出什么呢？
```
fn( 1, 2 ) === fn.call( undefined, 1, 2 ) === fn.apply( undefined, [1, 2] )
```
答案
- 严格模式 ==> ` this === undefined `
- 非严格模式 ==> ` this === undefined == 浏览器转化 ==> window `

#### 对象方法
## 示例二
```
let obj = {
    fn: function( a, b ){
        console.log( this )
    },
    child: {
        fn2: function(){
            console.log( this )
        }
    }
}
obj.fn( 1, 2 )
obj.child.fn2()
```
上述中的 ` this ` 会打印出什么呢？
```
obj.fn( 1, 2 ) === obj.fn.call( obj, 1, 2 ) === obj.fn.apply( undefined, [ 1, 2 ] )  ==> this === obj
obj.child.fn2() === obj.child.fn2.call( obj.child )  ==> this === obj.child
```
## 示例三
```
let arr = []
for( let i = 0; i < 3; i++ ){
    arr[ i ] = function(){
        console.log( this )
    }
}
let fn = arr[ 0 ]
arr[ 0 ]
fn()
```
上述中的 ` this ` 会打印出什么呢？
```
arr[ 0 ] ==> arr 对象{ 0: fn, 1: fn, 2: fn } ==> arr.0 ==> arr.0.call( this ) ==> this === arr
fn() ==> fn.call( undefined ) ==> this === undefined == 浏览器转化 ==> window
```

#### bind
` bind ` 的[用法](https://www.jianshu.com/p/e8ad9fdf8e07)在之前有讲过，这个 API 就是返回一个函数 + 绑定 ` this ` 
## 示例四
```
let obj = { a: 1 }
function fn(){
    console.log( this )
    console.log( this.a )
}
let newFn = fn.bind( obj )
newFn() 
```
上述中的 ` this ` 会打印出什么呢？
```
newFn() === fn.bind( obj )() ==> this === obj ==> this.a === 1
```
## 示例五
```
let obj = {
    container: document.querySeletor( 'body' )
    bind: function(){
        console.log( this )
        this.container.addEventListener( 'click', this.sayHello )
        this.container.addEventListener( 'click', this.sayHello.bind( this ) )
    },
    sayHello: function(){
        console.log( this )
    }
}
obj.bind()
```
上述中的 ` this ` 会打印出什么呢？
```
bind 中的 ` this ` ==> obj.bind.call( obj ) ==> this === obj
this.container.addEventListener( 'click', this.sayHello ) 当点击事件触发时，sayHello 打印出的 this ==> this === 绑定事件的元素（this.container） ==> this === obj.container
this.container.addEventListener( 'click', this.sayHello.bind( this ) ) 当点击事件触发时，sayHello 打印出的 this ==>  bind 绑定了 this[ this === obj ] ==> sayHello 打印出的 this === obj
```

#### 箭头函数
箭头函数没有 this + arguments
## 示例六
```
let obj = {
    fn1: function(){
        console.log( this )
    },
    fn2() {
        console.log( this )
    },
    fn3: () => {
        console.log( this )
    }
}
obj.fn1()
obj.fn2()
obj.fn3()
```
上述中的 ` this ` 会打印出什么呢？
```
obj.fn1() === obj.fn1.call( obj ) ==> this === obj
obj.fn2() === obj.fn2.call( obj ) ==> this === obj
fn1 的写法 === fn2 的写法
obj.fn3() === obj.fn3.call( 与 obj 同级 this ) ==> this === window
```

## 示例七
```
let obj = {
    init() {
        console.log( this )
        let prop = {
            init: () => {
                console.log( this )
            },
            bind() {
                console.log( this )
            } 
        }
        prop.init()
        prop.bind()
    }
}
obj.init()
```
上述中的 ` this ` 会打印出什么呢？
```
let obj = {
    init() {
        console.log( this )  // 2.  this === obj
        let prop = {
            init: () => {
                console.log( this )  // 4. this === prop 同级 this ==> this === obj 
            },
            bind() {
                console.log( this )  // 6. this === prop
            } 
        }
        prop.init()  // 3. init() 是箭头函数 ==> prop.init.call( prop 同级 this )
        prop.bind()  // 5. bind 不是箭头函数 ==> prop.bind.call( prop )
    }
}
obj.init()  // 1.  === obj.init.call( obj )
```

## 示例八
```
let obj = {
    x: console.log( this )
    fn1() {
        console.log( this )
        setTimeout( function(){
            console.log( this )
        }, 10 )
    },
    fn2() {
        console.log( this )
        setTimeout( () => {
            console.log( this )
        }, 20 )
    },
    fn3() {
        console.log( this )
        setTimeout( function(){
            console.log( this )
        }.bind( this ), 30 )
    },
    fn4: () => {
        console.log( this )
        setTimeout( () => {
            console.log( this )
        }, 40 )
    }
}
obj.fn1()
obj.fn2()
obj.fn3()
obj.fn4()
```
**obj.x 的值是什么？**

上述中的 ` this ` 会打印出什么呢？

```
let obj = {
    x: console.log( this )  // console.log( this ) 的值是 undefined ==> obj.x === undefined  其中的 this === window
}
```
**fn1()**
```
fn1() {
    console.log( this )   // 2. this === obj
    setTimeout( function(){
        console.log( this )  
    }, 10 )
    // 3. 等价于
    // function fn(){
    //     console.log( this )  // 5. this === undefined == 浏览器转化 ==> window
    // }
    // 过 10s 执行函数 fn
    // fn()   // 4. fn 非箭头函数  fn.call( undefined ) 
}
obj.fn1()  // 1. fn1 非箭头函数  obj.fn1.call( obj )
```
**fn2()**
```
fn2() {
    console.log( this )   // 2. this === obj
    setTimeout( () => {
        console.log( this )  // 5. this === 上级 this  ==> this === obj
    }, 20 )
    // 3. 等价于
    // () => {
    //     console.log( this )
    // }
    // 过 20s 执行箭头函数
    // 箭头函数()  // 4. 箭头函数 箭头函数.call( 上级this )
}
obj.fn2()  // 1. fn2 非箭头函数  obj.fn2.call( obj )
```
**fn3()**
```
fn3() {
    console.log( this )  // 2.  this === obj
    setTimeout( function(){
        console.log( this )
    }.bind( this ), 30 )
    // 3.  等价于
    // function fn(){
    //     console.log( this )  // 5. this === obj
    // }.bind( this )
    // 过 30s 执行函数 fn
    // fn.bind(this)  // 4. fn 非箭头函数 + bind 绑定 this[ this === obj ]
}
obj.fn3()  // 1. fn3 非箭头函数  obj.fn3.call( obj )
```
**fn4()**
```
fn4: () => {
    console.log( this )  // 2. this === obj 同级 this  ==> this === window
    setTimeout( () => {
        console.log( this )
    }, 40 )
    // 3. 等价于
    // () => {
    //     console.log( this )  // 5. this === window
    // }
    // 过 40s 后执行箭头函数
    // 箭头函数()   // 4. 箭头函数  箭头函数.call( 上级 this )
}
obj.fn4() // 1. fn4 箭头函数  obj.fn4.call( obj 同级 this )
```

## 总结
判断函数中 ` this ` 步骤
1. 查看调用函数的类型
2. 箭头函数
    - 箭头函数.call( 上级 this )
    - obj.箭头函数.call( obj 同级 this )
3. 非箭头函数
    - 非箭头函数.call( undefined )
    - obj.非箭头函数.call( obj )