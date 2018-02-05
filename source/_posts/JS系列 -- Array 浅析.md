---
title: JS系列 -- Array 浅析
date: 2018-02-05 22:34:07
tags: JavaScript
---
# 概述
数组是按次序排列的一组值的集合，它是一个**对象**。
数组可以存储任何类型的数据（数字，字符串，布尔值或者对象）

### 创建方法

##### 使用 Array 构造函数
![Array 构造函数](http://upload-images.jianshu.io/upload_images/9617841-6ea1c407cf635857.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 使用数组字面量表示法
数组字面量由一对包含数组项的方括号表示，多个数组之间以逗号隔开。
![Array 字面量](http://upload-images.jianshu.io/upload_images/9617841-335e259acf5fba52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 属性 length
表示数组的长度，即数组中元素的个数。
数组索引从 0 开始，索引上下限为 0 到 length-1 。
数组的 length 属性**不是只读**的，可以进行设置
![Array length 修改](http://upload-images.jianshu.io/upload_images/9617841-def1e376a8012efe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意：**
![Array length](http://upload-images.jianshu.io/upload_images/9617841-7e72fd6106cc9701.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
` arr02 ` 中 ` arr02[ 1 ] === undefined `，这个元素是存在的，只不过值为 undefined。

# 原型链
Array 是对象，所以 Array 有对象的一些方法
![Array 原型](http://upload-images.jianshu.io/upload_images/9617841-b4189f737381258e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以为数组添加属性，因为数组是对象。

# 检测数组
- ` Array.isArray() ` ==> 用于确定传递的值是否是一个 Array
- ` value instanceof Array ` ==> 使用 instanceof 操作符检测数组
- ` value.push() ` ==> 使用数组的特有方法，从而检测数组

# 方法 API
### Object 原型上的 API
![Object 原型上的 API](http://upload-images.jianshu.io/upload_images/9617841-94e2302778e6f341.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Array.prototype API
![Array 原型上的 API](http://upload-images.jianshu.io/upload_images/9617841-ebda095a9826c3f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 栈方法
栈是一种可以限制插入和删除项的数据结构。它是一种 LIFO（Last-In-First -Out，后进先出）的数据结构，栈中项的插入（推入）和移除（弹出）只发生在一个位置 ---- 栈的顶部。
- ` push() ` ==> 将一个或多个元素添加到数组的末尾，并**返回新数组的长度**。
- ` pop() ` ==> 从数组中删除最后一个元素，并**返回该元素的值**。此方法更改数组的长度。

#### 队列方法
队列数据结构的访问规则是 FIFO（Fitst-In-Fitst-Out，先进先出），队列在列表的末端添加项，在列表前端移除项。
- ` shift() ` ==> 从数组中删除第一个元素，并**返回该元素的值**。此方法更改数组的长度。
-  ` unshift() ` ==> 将一个或多个元素添加到数组的开头，并**返回新数组的长度**。

#### 重排序方法
重排序方法 ` sort() ` 和` reverse() ` 的**返回值是经过排序之后的数组**，这两种方法都会**改变原数组**。

-  ` sort() ` ==> 在适当的位置对数组的元素进行排序，并**返回数组**。默认排序顺序是根据**字符串Unicode码点**。
    ![Array sort 排序](http://upload-images.jianshu.io/upload_images/9617841-4bd79d3863269eaf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    ![Array sort 排序](http://upload-images.jianshu.io/upload_images/9617841-b07b786adcc4b08e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-  ` reverse() ` ==> 将数组中元素的位置颠倒，并**返回该数组的引用**。第一个数组元素成为最后一个数组元素，最后一个数组元素成为第一个。
![Array reverse](http://upload-images.jianshu.io/upload_images/9617841-03c659fb37fe7170.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 操作方法
-  ` concat() ` ==> 用于合并两个或多个数组。此方法不会更改现有数组，而是**返回一个新数组**。

-  ` slice() ` ==> **返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象**，原数组不会被修改。
![Array slice()](http://upload-images.jianshu.io/upload_images/9617841-f46d0e64ba879c47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-  ` splice() ` ==> 通过删除现有元素和/或添加新元素来更改一个数组的内容。
**返回由被删除的元素组成的一个数组**。
**删除了一个元素** ==> **返回只包含一个元素的数组**。
**没有删除元素** ==> **返回空数组**。
**参数：** ` array.splice(start [ , deleteCount [, item1 [, item2, ... ] ] ]) ` 
![Array splice API](http://upload-images.jianshu.io/upload_images/9617841-5d25a212665cf00a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 位置方法
-  ` indexOf() ` ==> **返回在数组中可以找到一个给定元素的第一个索引**，如果不存在，则**返回-1**
-  ` LastIndexOf() ` ==> **返回指定元素在数组中的最后一个的索引**，如果不存在则**返回 -1**。从数组的后面向前查找

#### 迭代方法
-  ` forEach() ` ==> 对数组的每个元素执行一次提供的函数。**返回 undefined**
参数：**回调函数**和可选的执行回调函数时用作this的值，**回调函数**接受三个值：
    1. 数组中正在处理的当前元素
    2. 数组中正在处理的当前元素的索引
    3. forEach()方法正在操作的数组
![Array forEach](http://upload-images.jianshu.io/upload_images/9617841-2ecfa4be91a212d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-  ` map() ` ==> 创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。**返回一个新数组，每个元素都是回调函数的结果**
**和 ` forEach ` 作用一样，只是返回一个新数组**
![Array map](http://upload-images.jianshu.io/upload_images/9617841-596d0557d2e52a1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-  ` every() ` ==> 测试数组的所有元素是否都通过了指定函数的测试。**返回布尔值**
   
-  ` some() ` ==> 测试数组中的某些元素是否通过由提供的函数实现的测试。**返回布尔值**
   
![Array every + some](http://upload-images.jianshu.io/upload_images/9617841-f3f669c4e73662af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


-  ` filter() ` ==> 创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 **返回一个新的通过测试的元素的集合的数组**

![Array filter](http://upload-images.jianshu.io/upload_images/9617841-36de13831ddd2bf4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 归并方法
-  ` reduce() ` ==> 对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。**返回函数累计处理的结果**
-  ` reduceRight() ` ==> 接受一个函数作为累加器（accumulator）和数组的每个值（从右到左）将其减少为单个值。**返回函数累计处理的结果**