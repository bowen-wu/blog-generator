---
title: JS系列 -- Array 浅析之相关 API
date: 2018-01-30 09:02:22
tags: JavaScript
---
## 判断数组
```
Array.isArray( arguments )
```
## 原型
**实例化对象的 ` __proto__ ` 指向构造函数的 ` prototype `**
```
typeof Array === 'function'

Array instanceof Function === true
Array.__proto__ === Function.prototype

Array instanceof Object === true
Array.__proto__.__proto__ === Object.prototype 

let arr = []
arr.__proto__ === Arr.prototype
arr.__proto__.__proto__ === Object.prototype
arr.__proto__.constructor === Array
```
## 需求
#### 需求一
```
let arr1 = ['a']
let arr2 = [ 'b' , 'c' ]
数组 arr1 ==> [ 'a', 'b', 'c' ]
数组 arr2 不变
```
###### 解决方法
```
1. arr1 = arr.concat(arr2)
2. arr2.forEach( (item) => {arr1.push(item)} )
3. arr1.push( ...arr2 )
4. [].push.apply( arr1, arr2 )
```

#### 需求二
```
function max(){
    // 代码
}
max(1,2,3) // 3
max( 1,4 ) // 4
```
###### 解决方法
```
1. return Math.max(...arguments)
2. return Math.max.apply( null, arguments )
```
#### 需求三
将 ` arguments ` 转化为 ` Array `
###### 解决方法
```
1. args = Array.from( arguments )
2. args = [].slice.call( arguments )
```
#### 需求四
```
function repeat( str, num ){
    // 代码
}
repeat( 'abc', 3 ) // 'abcabcabc'
```
###### 解决方法
```
return Array( num + 1 ).join( str )
```
#### 需求五
```
arr = [ 'a', 'b', 'c' ]
判断 'b' 在不在 arr 里面
```
###### 解决方法
```
arr.indexOf( 'b' ) !== -1  // true ==> 在   false ==> 不在
arr.includes( 'b' )   // 直接返回 true | false
```
#### 需求五升级
```
let arr = [ {name: 'xiaoming'}, {name: 'xiaozhang'} ]
查看数组中是否有叫小明的人
```
###### 解决方法
```
1. arr.forEach((item) => {
	if(item.name === 'xiaoming'){
		console.log('true')
	}
   })
// forEach 方法返回 undefined ，没有想到怎么取得它的结果，目前只能 console.log('true')
2. let it = arr.find( (item) => {
    return item.name === 'xiaoming'
  })
  console.log(it)  // {name: "xiaoming"}
3. let it = arr.findIndex((item) => {
	return item.name === 'xiaoming'
    })
    console.log(it)  // 0
```
#### 需求六
```
let arr = [ 20, 30, 40 ]
使得 arr 中每个元素 +5
arr ==> [ 25, 35, 45 ]
```
###### 解决方法
```
1. arr.map( (item) => {
    return item + 5
  })
2. arr.forEach( (item, index, ctx) => {
    ctx[ index ] = item + 5
   })
```
#### 需求七
```
let obj = { name: 'xiaoming', age: 20, id: 123456789 }
function validPerson( person ){
    // 如果有 名字 | id 就有效
    // 代码
}
```
###### 解决方案
```
1. if(person.hasOwnProperty( 'name' ) || person.hasOwnProperty( 'id' )){
      return true
   }
    return false
2. 
let obj = { name: 'xiaoming', age: 20, id: 123456789 }
let validFileds = [ 'name', 'id' ]
function validPerson( person ){
    return validFileds.some( (item) => {
        return person.hasOwnProperty( item )
    })
}
3. 
let obj = { name: 'xiaoming', age: 20, id: 123456789 }
let validFileds = [ 'name', 'id' ]
function validPerson( person ){
    return validFileds.some( Object.prototype.hasOwnProperty, person )
}
```