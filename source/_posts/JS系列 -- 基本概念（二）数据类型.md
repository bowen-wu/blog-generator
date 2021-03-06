---
title: JS系列 -- 基本概念（二）数据类型
date: 2018-01-30 08:53:32
tags: JavaScript
---
# 概述
1. 基本数据类型（简单数据类型）：
    - 数值
    - 字符串
    - 布尔值
    - undefined
    - null
    - Symbol 
2. 复杂数据类型：
    - Object  ==>  由一组无序的键值对组成，即哈希。分为 **狭义对象** **数组** **函数**
3. 原始类型：字符串 数值 布尔值
4. 合成类型：对象
5. 特殊值：undefined null

# 判断类型
### 方法：
1. typeof 运算符：返回一个值的数据类型(**字符串**)
      - "undefined" ==> 如果这个值  **未定义**
      - "boolean" ==> 如果这个值是  **布尔值**
      - "string" ==> 如果这个值是  **字符串**
      - "number" ==> 如果这个值是  **数值**
      - "Object" ==> 如果这个值是   **对象** 或 **null**
      - "function" ==> 如果这个值  **函数**
2. instanceof 运算符
3. Object.prototype.toString方法
    
### 注意：
1. 没有 Array 类型
2. 对**未初始化**的变量执行 typeof 操作符会返回 undefined 值
3. 对**未声明**的变量执行 typeof 操作符会返回 undefined 值
  
# 数值
### NaN
非数值，是一个特殊的**数值**（typeof NaN  ==>  number）
- 任何涉及 NaN 的操作都会返回 NaN
- NaN 与任何值都不相等，包括 NaN 本身

### 转换类型（3函数 2操作符）
- ` Number() ` ==> 可以用于任何函数
- ` parseInt() ` ==> 用于将字符串转换为数值 **` parseInt("") //NaN `**
- ` parseFloat() ` ==> 用于将字符串转换为数值
- 一元 ` + ` 操作符 ==> 等价于 ` Number() `
- ` ' - 0 ' `  //任意一个类型减去0

**` parseInt('s') ==> NaN `**

# 字符串

### 概述
字符串默认只能写在一行内，分成多行将会报错。
- 如果长字符串必须分成多行，可以在每一行的尾部使用**反斜杠**。
**注意**：反斜杠的后面必须是换行符，而不能有其他字符（比如空格），否则会报错。
- 连接**运算符（+）**可以连接多个单行字符串，将长字符串拆成多行书写，输出的时候也是单行

### 特点
字符串是不可变的，字符串一旦创建，值就不能改变，要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量

### length属性
返回字符串的长度，该属性也是无法改变的

### 字符字面量（转义符）
反斜杠（\）在字符串内有特殊含义，用来表示非打印，所以又称为字符字面量（转义符）。
```
\0 ：null（\u0000）
\b ：后退键（\u0008）
\f ：换页符（\u000C）
\n ：换行符（\u000A）
\r ：回车键（\u000D）
\t ：制表符（\u0009）
\v ：垂直制表符（\u000B）
\' ：单引号（\u0027）
\" ：双引号（\u0022）
\\ ：反斜杠（\u005C）
```
如果在非特殊字符前面使用反斜杠，则反斜杠会被省略。

### 字符串与数组
字符串可以被视为字符数组，因此可以使用数组的方括号运算符，用来返回某个位置的字符（位置编号从0开始）。
无法改变字符串之中的单个字符。

### 类型转换
转换为字符串
1. 一元 + 号操作符   
2. String()     
3. toString()

# 布尔值
 **falsy** ： 在 Boolean 上下文中认定可转换为 false 的值 ( 5个假值 ) 
```
false
0
NaN
''
null
undefined
```

**空数组（[]）和空对象（{}）对应的布尔值，都是true。**

### 转换类型
转换为布尔值
1. Boolean()
2. ` !! `    //取反再取反

# null 和 undefined 区别
- null是一个表示**“空”的对象**，转为数值时为0。
- null表示**空值**，即该处的值现在为空。调用函数时，某个参数未设置任何值，这时就可以传入null，表示该参数为空。

- undefined是一个表示”此处无定义”的原始值，转为数值时为NaN。返回undefined的情景。
    ```
    // 变量声明了，但没有赋值
    var i;
    i // undefined

    // 调用函数时，应该提供的参数没有提供，该参数等于 undefined
    function f(x) {
      return x;
    }
    f() // undefined

    // 对象没有赋值的属性
    var  o = new Object();
    o.p // undefined

    // 函数没有返回值时，默认返回 undefined
    function f() {}
    f() // undefined
    ```