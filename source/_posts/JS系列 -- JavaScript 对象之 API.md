---
title: JS系列 -- JavaScript 对象之 API
date: 2018-02-07 19:17:07
tags: JavaScript
---
# 概述
之前介绍了 [《JavaScript 对象基础》 ](https://bowen-wu.github.io/2018/01/10/JS%E7%B3%BB%E5%88%97%20--%20JavaScript%20%E5%AF%B9%E8%B1%A1%E5%9F%BA%E7%A1%80/)，现在我们来介绍一下挂载在 ` Object.prototype ` 上的属性

# Object.prototype API
` Object.prototype ` 表示对象的原型对象
` Object.prototype ` 属性的属性特征
```
writable ==> 是否可写 ==> false(默认)
enumerable ==> 是否可枚举 ==> false(默认)
configurable ==> 是否可配置 ==> false(默认)
```

### Object.create()
创建一个空对象，空对象的原型指向参数
```
Object.create( proto[, propertiesObject] ) // 第二参数为一个对象，可以写入新对象的属性 + 描述符
```
##### 使用 Object.create() 实现继承
![Object.create 实现继承](http://upload-images.jianshu.io/upload_images/9617841-985125c7f108b498.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Object.setPrototypeOf() 
设置一个指定的对象的原型到另一个对象或 ` null `
```
Object.setPrototypeOf( obj, prototype ) // 将对象 obj 的原型设置为 prototype（原型 | null）
obj = Object.create( prototype )  === Object.setPrototypeOf( obj, prototype )
```

### Object.defineProperty() | Object.defineProperties()
这两个方法都是在一个对象上定义新属性或修改现有属性，并返回对象
```
Object.defineProperty( obj, prop, descriptor )
Object.defineProperties( obj, props )
```
##### 属性描述符
属性描述符分为**数据描述符** 和 **存取描述符**。描述符必须是二者之一
- 数据描述符 ==> value + writable ==> 具有值的属性，该值可能是可写的，也可能是不可写的
- 存取描述符 ==> set + get
###### 描述符可选键值
- configurable ==> 是否可配置 ==> false(默认)

- enumerable ==> 是否可枚举 ==> false(默认)

- writable ==> 是否可写 ==> false(默认)

- value ==> 属性对应的值

- get ==> 一个给属性提供 getter 的方法，如果没有 getter 则为 undefined。该方法返回值被用作属性值，默认为 undefined

- set ==> 一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。该方法接受唯一参数，并将该参数的新值分配给该属性，默认为 undefined
![属性描述符](http://upload-images.jianshu.io/upload_images/9617841-3a71e414e2343590.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果对象属性有 getter + setter ，那么调用时走 getter，赋值时走 setter，getter + setter 作用：
    1. 保护私有变量
    2. 响应式开发 | 双向数据绑定 | MVVM （` Object.definedProperty() + get + set `）

```
let person = {}
let ageValue = 18
Object.definePrototype( person, 'name', {
    value: xxx,
    configurable: false,  // 如果不写默认 false
    writable: true,
    enumerable: true
})

Object.definePrototype( person, 'age', {
    configurable: false,  // 如果不写默认 false
    enumerable: true,
    get : function(){
        return ageValue;
    },
    set : function( newValue ){
        ageValue = newValue;
    },
})
person.age   // 18
person.age = 19
person.age  // 19

Object.defineProperties( person, {
    'property1' : {
        value: xxx,
        configurable: false,  // 如果不写默认 false
        writable: true,
        enumerable: true
    },
    'property2' : {
        value: xxx,
        configurable: false,  // 如果不写默认 false
        writable: true,
        enumerable: true
    }
})
```
### Object.getOwnPropertyDescriptor()
返回制定对象上一个自有属性对应的属性描述符
```
Object.getOwnPropertyDescriptor( obj, prop )
```
### Object.getOwnPropertyNames()
返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括 Symbol 值作为名称的属性）组成的数组

### [Object.keys() | Object.values()](https://bowen-wu.github.io/2018/01/10/JS%E7%B3%BB%E5%88%97%20--%20JavaScript%20%E5%AF%B9%E8%B1%A1%E5%9F%BA%E7%A1%80/)

### Object.prototype.hasOwnProperty()
返回一个布尔值，指示对象自身属性中是否具有指定的属性
```
obj.hasOwnProperty( prop )
```

# 使用 get + set 实现需求
给定一个对象 obj，返回一个新对象，新对象具有对象 obj 的所有属性，并且给新对象每个属性重新赋值都会触发修改函数，在下方代码处填写代码
```
function getReactiveObj( obj, cb ){
    // 代码 code
}
let obj = { a: 'a', b: 'b' }
function fn( prop,val ){
    console.log( `${ prop }属性的值变为${ val }` )
}
let newObj = getReactiveObj( obj, fn )
console.log( newObj.a )  // a
console.log( newObj.b )  // b
newObj.a = 'd'  // a属性的值变为d
console.log( newObj.a )  // d
console.log( obj )  // { a: 'a', b: 'b' }
```
解决方案
```
function getReactiveObj( obj, cb ){
    let result = {}
    Object.keys( obj ).forEach( ( item ) => {
        let value = obj[ item ]
        Object.defineProperty( result, item, {
            get() {
                return value
            },
            set( val ) {
                cb.call( null, item, val )
                value = val
            } 
        } )
    })
    return reault
}
```


# 实现查询参数
把一个 JS 对象转化成符合查询参数的字符串
```
function parseToQuery(){
    // 代码
}
parseToQuery({ id: 100, name: 'hello', groups: 'xxx' })  // 'id=100&name=hello&groups=xxx' 
parseToQuery({ id: 100, name: 'hello', groups: [ 'xxx', 'yyy' ] })  // 'id=100&name=hello&groups=xxx&groups=yyy' 
```
### 实现无数组方法一
```
function parseToQuery( obj ){
    let arr = []
    for( let key in obj ){
        arr.push( encodeURIComponent( key ) + '=' + encodeURIComponent( obj[ key ] ) )
    }
    return arr.join( '&' )
}
```
##### encodeURIComponent()
对统一资源标识符（URI）的组成部分进行编码的方法
```
encodeURIComponent( str )   // URI 的组成部分 
```
### 实现无数组方法二
```
function parseToQuery( obj ){
    let str = ''
    Object.keys( obj ).forEach( ( item ) => {
        var str1 = ` ${ item }=${ obj[ item ] }& `
        str = str.concat( str1 )
    })
    return str.slice( 0, str.length-1 )
}
```
### 实现无数组方法三
```
function parseToQuery( obj ){
    return Object.keys( obj ).map( ( key ) => {
        `${ key }=${ obj[ key ] }`).join( '&' )
    })
}
```
### 实现有数组方法
```
function parseToQuery( obj ){
    let result = ''
    for( let key in obj ){
        if( obj[ key ] instanceof Array ){
            obj[ key ].forEach( (item) => {
                result += key + '=' + value + '&'
            })
        }else{
            result += key + '=' + obj[ key ] + '&'
        }
    }
    return result.substring( 0, result.length-1 )
}
```