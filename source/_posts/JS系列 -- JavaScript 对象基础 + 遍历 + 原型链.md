---
title: JS系列 -- JavaScript 对象基础 + 遍历 + 原型链
date: 2018-01-10 12:52:55
tags: JavaScript
---
# 概述
对象就是一组数据和功能的集合。它相当于哈希（键值对），使用对象属性有 ` [ ] ` 操作（` obj[ 'name' ] `）和点操作（` obj.name `）
- 键名 ==> **字符串**（ES6 又引入了 Symbol 值也可以作为键值）
- 键名 ==> **数值** ==(自动转为)==> **字符串**
- 键名 ==> 不符合标识名的条件（比如第一个字符为数字，或者含有空格或运算符），且也不是数字，则必须加上**引号**，否则会报错
```
obj["name"]   // 必须带有 " " ，如果不带 " " JS 首先会找 name 的值，但是 name === undefined ，自然就报错了 
```
- 键名 ==> 满足标识符（变量名）的规则 ==> ` obj.name `(可以使用点操作符)
- delete 操作符删除对象属性
    ```
    delect obj["name"]
    obj.name   // undefined(无value)
    name in obj   // false(无key)
    ```
# 对象声明
1. 
    ```
    var obj1 = {} 
    ```
2. 
    ```
     var obj2 = new Object();  //obj1 和 obj2 没有区别，但是 obj1 === obj2 是 false ，因为存储在 Stsck(栈内存) 中的地址不同
    ```
3. 
    ```
    var obj3 = Object.create(Object.prototype)
    ```



# [基本包装类型：Boolean()  Number()  String()](https://bowen-wu.github.io/2018/01/10/JS%E7%B3%BB%E5%88%97%20--%20%E5%9F%BA%E6%9C%AC%E5%8C%85%E8%A3%85%E7%B1%BB%E5%9E%8B/)
基本包装类型有属性 `__proto__` ，这个属性引用（指向）了在 Heap( 堆内存 ) 的一个地址，这个地址表示**基本包装类型对应的 prototype（原型）**。基本包装类型有**重写属性**（表示重写属性从而覆盖 **Object 的 prototype**）
- Number()
![基本包装类型 Number](http://upload-images.jianshu.io/upload_images/9617841-6e664f59886709f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- String()
![基本包装类型 String](http://upload-images.jianshu.io/upload_images/9617841-5c889bdc59874e0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- Boolean()
![基本包装类型 Boolean](http://upload-images.jianshu.io/upload_images/9617841-4911258278ef065e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 遍历对象
### for ... in ... 遍历 自身 + 原型
通过 Objct.prototype.hasOwnProprety 筛选出自身属性
```
for( let key in obj ){
    if( obj.hasOwnProperty( key ) ){
        console.log( obj[ key ] )  // 此时必须使用 [] ，key 表示的是字符串
    }
}
```

![Object for...in 遍历](http://upload-images.jianshu.io/upload_images/9617841-c3c19ca702bb6cbd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Object.keys() + Object.values() 遍历 自身可枚举属性
` Object.keys() ` + ` Object.values() ` 这两种方法的返回值都是一个数组
```
Object.keys(obj).forEach( ( item ) => {
    console.log( item )
})
```
![Object.keys() + Object.values() 遍历](http://upload-images.jianshu.io/upload_images/9617841-fd35328e99b0a602.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Object.getOwnPropertyNames() 遍历 自身可枚举 + 自身不可枚举
返回由指定对象的所有自身属性的属性名（包括不可枚举但不包括 Symbol 值作为名称的属性）组成的数组
```
Object.getOwnPropertyNames( obj ).forEach( (item) => {
    console.log( obj[ item ] )
})
```

# __proto__ + prototype 
实例化对象的 ` __proto__ ` 指向构造函数的 ` prototype `，即**` 对象.__proto__  ===  函数.prototype `**
```
"1".__proto__ === String.prototype
1.__proto__ === Number.prototype
true.__proto__ === Boolean.prototype
```
### 测试
```
let arr = []
arr.__proto__ === ??? 
Array.__proto__ === ???
typeof Array.prototype  ???
Array.prototype.__proto__ === ???
```
### 答案
```
arr.__proto__ === Array.prototype
Array.__proto__ === Function.prototype
typeof Array.prototype  // 'Object'
Array.prototype.__proto__ === Object.prototype
```

## 对象属性赋值 + 查找
对象属性赋值是在自身属性赋值，不会修改原型链，而查找会在原型链中查找

# new 的实现
` var 对象 = new 函数 `
### 方法一：Object.create()
```
function _new(){
    let constructor = arguments[0]
    let obj = Object.create( constructor.prototype )
    constructor.apply( obj, [].slice.call( argumnets, 1 ))
    return obj
}
```
## 方法二：Object.setPrototypeOf()
```
function _new(){
    let constructor = arguments[0]
    let obj = {}
    Object.setPrototypeOf( obj, constructor )
    constructor.apply( obj, [].slice.call( argumnets, 1 ))
    return obj
}
```

# instanceof 
` instanceof ` 是在原型链中查找是否是其实例（instance）
### instanceof 实现
```
function _instanceof( instance, fn ){
    let i = instance.__proto__
    while( i ){
        if( i === fn.prototype ){
            return true
        }
        i = i.__proto__
    }
    return false
}
```

# 继承
实现继承主要是要实现原型的私有属性 + 公共属性

### 私有属性继承
```
call
```

### 公共属性继承
```
Object.create()
Object.setPrototypeOf()
```

### 私有属性 + 公共属性继承
```
new
```
[继承示例](https://www.jianshu.com/p/912335a9a175)

## 相关知识点
最简单的克隆
```
result = JSON.parse( JSON.stringify( obj ) )
result = Object.assign({}, obj)
```