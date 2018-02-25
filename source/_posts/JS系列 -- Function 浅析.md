---
title: JS系列 -- Function 浅析
date: 2018-01-11 20:04:06
tags: JavaScript
---
# 概述
函数是一个可以执行代码的对象。每个函数都是 Function 类型的实例，并且都与其他引用类型一样具有属性和方法。

![Function](http://upload-images.jianshu.io/upload_images/9617841-4afc8efd99ed4324.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

每个 Function 的实例的 `__proto__` 都指向了 Function.prototype(原型)，函数原型（Function.prototype）的 `__proto__` 指向了 Object.prototype(对象的原型)

# 定义函数
1. 函数声明
    ```
    function name( param1[ , param2[, ...] ] ){
        return
    }
    ```
    **如果 `return` 省略，那么浏览器将自动添加 `return undefined`**
    `function` 是关键字，它只能用来声明函数，而 `var` 则可以用来声明7    种数据类型（number、string、boolean、null、undefined、symbol、    object）中的任意一种。

2. 函数表达式
    ```
    var x = function( param1[ , param2[, ...] ] ){
        return 
    }
    ```
    匿名函数需要赋给变量，之后才能调用。**这个匿名函数称为函数表达式**
3. 混合式
    ```
    var fn = function name( param1[ , param2[, ...] ] ){
        return 
    }
    ```
    混合式中，函数名 name 只在函数体内部有效，指代函数表达式本身，在函数体外部无效。
4. Function 构造函数
    最后一个参数被始终看成函数体，而前面的参数则枚举了新函数的参数。
    ```
    var fn = new Function( 'param1'[ , 'param2'[,' ...'] ] , functionBoby ){
        return 
    }
    ```
    **例子：**
    ```
    let n = 1;
    let fn = new Function( 'x' , 'y' , 'return x + ' + n + ' + y' );
    fn( 1,2 )  //4，此时的 n 是1
    ```
5. 箭头函数（ES6）
    ```
    （param1[ , param2[, ...] ]）=> { return  }
    ```
### 形参和实参
- parameters ==> 形参，定义时的占位符
- arguments ==> 实参，调用时传入的参数

### 说明
- 函数会在执行完 return 语句之后停止并立即退出，因此，位于 return 语句之后的任何代码都永远不会执行。
- 没有传递值的命名参数将自动被赋值 undefined
- 所有参数传递的都是值，不可能通过引用传递参数
- 验证函数，使用 ` typeof xxx === 'function' `

# 函数声明和函数表达式
- 函数声明
    浏览器解析器会率先读取函数声明（**声明前置**），并使其在执行任何代码之前可用
- 函数表达式
    必须要等到浏览器解析器执行到它所在的代码行，才会被解析执行

# 声明前置
先声明变量，后声明 function，如果 function 名字和 var 声明名字一样
 function 优先。如果赋值就按照赋值的类型。

![声明前置](http://upload-images.jianshu.io/upload_images/9617841-b685a2f72fd44407.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# API
### 自身 API
![自身属性 API](http://upload-images.jianshu.io/upload_images/9617841-03cb79b4dd1cd59d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ` name ` ==> 返回函数实例的名称

![fn.name](http://upload-images.jianshu.io/upload_images/9617841-43d062bc839e7d48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ` length ` ==> 指明函数的**形参**个数

### Function.prototype API
![Function.prototype API](http://upload-images.jianshu.io/upload_images/9617841-b7e392157560ccdd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ` call() ` ==> 调用一个函数, 其具有一个指定的this值和分别地提供的参数(参数的列表)
    **作用：**
    1. 改变 this 值
    ![改变 this 值](http://upload-images.jianshu.io/upload_images/9617841-248eb360eed5d5e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    2. 操作参数

    **语法：**
    ```
    fun.call(this, arg1, arg2, ...)
    ```

    **参数：**
    1. this ：函数运行时的指定的 this 值。
        - **非严格模式**下，若 this 为 null 或者 undefined ，则 this 自动指向**全局对象（global[ window ]）**，同时值为原始值(数字，字符串，布尔值)的 this 会**指向该原始值的自动包装对象**。
        ![非严格模式](http://upload-images.jianshu.io/upload_images/9617841-91e0f7d177fcb401.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

        - **严格模式**下，若 this 为 undefined ，则 **this === undefined**
        ![严格模式](http://upload-images.jianshu.io/upload_images/9617841-8ed9e92d37439649.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
        此处 call 的时候指明了 this === 1，例子不明显，应该 fn.call()，此时便可以打印出 undefined

    2. arguments ：伪数组，元素为函数的参数
        - 函数中的参数在内部是用一个伪数组（`arguments`）来表示，在函数内部通过 `arguments` 对象来访问参数从而获取传递给函数的每一个参数。
        - `arguments` 可以被用作被调用对象的所有未指定的参数。可以使用arguments来把所有的参数传递给被调用对象。 
        - `arguments` 对象的长度是由传入的参数个数决定的，不是由定义函数时的命名参数的个数决定的。

- ` apply() ` ==> 调用一个函数, 其具有一个指定的`this`值，以及作为一个数组（或**类似数组的对象**）提供的参数。
     **作用：**和 call() 一样
    1. 改变 this 值
    2. 操作参数

    **语法：**
    ```
    fun.apply(this, arguments)
    ```

    **使用实例：**
    1. 实例1：**不关心 this ，所以将 this 传为 null**
    ![apply 实例1](http://upload-images.jianshu.io/upload_images/9617841-ca8c95bbb5abd53b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    2. 实例2：**关心 this ，this === arr1**
    ![Apply 实例2](http://upload-images.jianshu.io/upload_images/9617841-3b3a79c946e1e7c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  **说明：` call() ` 和 ` apply() ` 的区别**
    `apply()` 与 `call()` 作用相同，不同之处在于**提供参数的方式**。
    `apply()` 使用参数数组，**可以将数组里的元素拆分传入参数**
    `call()` 使用一组参数列表，一个一个传递参数
    `apply `可以使用数组字面量，也可以使用 `arguments `对    象， `arguments` 是一个函数的局部变量。 



- ` bind() ` ==> 创建一个新的函数, 当被调用时，将其 ` this ` 关键字设置为提供的值，**返回新创建的函数**。在调用新函数时，在任何提供之前提供一个给定的参数序列。
    **作用：**
    1. 切换上下文（this）
    2. 科里化

    **语法：**
    ```
    fun.bind( this [ , arg1 [ , arg2 [ , ... ] ] ] )
    ```
    **使用实例：**
    1. 实例1：**基本用法**
    ![bind 基本用法](http://upload-images.jianshu.io/upload_images/9617841-f825f2fa123c4db2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    2. 实例2：**实际应用**
    ![bind 实际应用](http://upload-images.jianshu.io/upload_images/9617841-71e4fff179f8a3f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    3. 实例3：**利用 this + apply 实现 bind**
    ![重构 bind](http://upload-images.jianshu.io/upload_images/9617841-ca4c2e02d2132d0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# new
1. 创建一个新对象 obj ，对象 ` __proto__ ` 指向构造函数的 ` prototype `
    ```
    对象. __proto__ === 构造函数.prototype
    ```
2. 调用 call()/apply()/bind()
  
# Call Stack（调用栈）
栈是一种数据结构，特点是**先进后出**，js 代码执行时都遵循 Call Stack 的规则。如果压栈太多会导致错误（Stack Overflow[ 栈溢出 ]）

# 作用域 和 作用域链
scope（范围、视野、眼界）

**如果在函数作用域中没有写 `var` 直接写 ` a = 3 `**
    1. 优先赋值
    2. 沿着作用域链寻找 a 的声明
    3. 若作用域链中没有 a 的声明，则声明全局变量并赋值

# 变量提升
将作用域链中首先要做**变量提升**
###### 例子1：
```
var a = 1;
function fn(){
    console.log( a );
    var a = 2;
}
fn.call();  //  a === undefined
```
    
###### 例子2：
```
var a = 1;
function fn1(){
    console.log( a );  //  a === undefined
    var a = 2;
    fn2.call();
}
function fn2(){
    console.log( a );  //  a === 1
}
fn1.call();
```
###### 例子3：
```
var obj = { name: ' obj ' }
function fn1(){
    function fn2(){
        console.log( this );   // this === window 
    }
    fn2.call();
}
fn1.call( obj );
```
###### 例子4：
```
var liTags = document.querySelectAll( ' li ' );
for( let i = 0, len = liTags.length; i < len; i++ ){
    liTags[ i ].onclick = function(){
        console.log( i )  // 打印出的 i 都是最后的数字
    }
}
```
# this 值

this 的调用主要有两种方式，一种是函数（function），另一种是作为对象的方法（methods）
### 判断 this 值就看函数怎么被调用，之后转化为 call 形式
###### 例子1：作为对象的方法调用
```
function fn(){
    console.log(this)
}
let obj = {
    a: 'a',
    fn: fn
}
obj.fn()  // this === obj
```
###### 例子2：作为对象属性的深层次嵌套
```
function fn(){
    console.log(this)
}
let obj = {
    a: 'a',
    wrapper: {
        b: 'b',
        fn: fn
    }
}
obj.wrapper.fn()  // this === { b: 'b', fn: fn } === obj.wrapper
```
###### 例子3：
```
let obj = {
    a: 'a',
    fn () {
        console.log(fn)
    }
}
function fncb(cb){
    cb()
}
fncb(obj.fn)  // this === 全局变量
```

# 闭包
如果一个函数使用了它范围外的值，那么这个函数 + 这个变量就叫做闭包。
  
![声明变量](http://upload-images.jianshu.io/upload_images/9617841-19f3603e86a9f96d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 立即调用函数
window下有很多全局属性
![window](http://upload-images.jianshu.io/upload_images/9617841-e883bce645e9a47e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
当定义变量的时候，有可能不小心覆盖了 window 的全局属性，为了避免覆盖 window 上的属性，可以：
1. 不用全局属性
2. 用局部变量
3. 使用立即调用函数，从而使用局部变量
    ```
    () => { }.call()
    () => { }()
    ```
4. 但是如果像上述写法，浏览器会报错，所以要这么写：
    ```
    (() => { }.call() )
    (() => { } ).call()
    -() => { }.call()
    +() => { }.call() 
    ！() => { }.call()
    ～() => { }.call()
    { let  }  // let 作用域在块级作用域中
    ```

# 套路
1. 如果同一个函数被多次声明，后面的声明就会覆盖前面的声明

2. 递归：函数调用自身

3. JavaScript将函数看做一种值，函数只是一个可以执行的值

4. 当**函数表达式**和**函数声明**同时有的时候
    ![function](http://upload-images.jianshu.io/upload_images/9617841-f712689a1b6da638.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. 不能在条件语句中声明函数（变量提升，是的 if 无效）

6. **函数执行时所在的作用域，是定义时的作用域，而不是调用时所在的作用域。**

7. 参数**传值传递**，内存中体现**原始值的拷贝**。
参数**传址传递**（复杂类型的值），内存中体现**原始值的地址**，唯一一种不改变参数的是**替换掉整个参数，这时不会影响到原始值**（添加属性或方法会影响到原始值）

8. 如果有同名的参数，则取最后出现的那个值

9. eval命令的作用是，**将字符串当作语句执行**。eval没有自己的作用域，都**在当前作用域内执行**，因此可能会修改当前作用域的变量的值，造成安全问题。为了防止这种风险，JavaScript 规定，如果使用**严格模式**，**eval内部声明的变量，不会影响到外部作用域**