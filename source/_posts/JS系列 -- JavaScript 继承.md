---
title: JS系列 -- JavaScript实现继承方式
date: 2017-12-21 23:00:08
tags: JavaScript
---
JavaScript实现继承方式
# 1. new
`Student.prototype = new Person();`
得到一个 Person 实例，并且这个实例指向构造器的 prototype 属性和调用了构造函数，因为调用的构造函数，而 Student 只是一个类，并没有实例化，只为了继承，调用构造函数创建一个实例，参数的问题就不容易解决。

**例如**：当 Person() 中有一个 name 属性时，而 Student 上也有 name 属性时，当 
Student.prototype = new Person(); Person 上的 name:undefined ,并且 Student 永远不会用到 Person 上的 name 性，如果 Person 上有很多这样的属性情况可想而知。

**new 操作符所做的事情(var a = new A())**
a.首先生成一个空对象 a = {} ，它是 Object 的实例
b.设置 a 的原型链 a.__proto__ = A.prototype 
c.A.call(a), A 的 this 指向 a,并执行 A 的函数体
d.判断 A 的返回值类型，
如果   '没有返回值' 或者 '返回值类型为值类型' 返回 this 即 a （this === a），
如果有返回值，并且返回值类型为引用类型，就返回这个引用类型的对象，替换掉 a 
```
        var A = function(){
            return this.__proto__;
        }
        var a = new A();
        console.log(a) //object{}
```
解释：首先 this === a， 但是 A 返回的是 this.__proto__ (a.__proto__), this.__proto__ 指向了原型对象 Object() 构造函数创建的 object 对象，它是一个引用类型，它替换掉 a ，所以这里的变量 a 是一个指针，指向它的原型。

`console.log(A.prototype === a); //true`

此时 A 构造函数的原型和 a 指针（指向 a.__proto__）是同一个对象

# 2. Object.create()

`var obj = Object.create({ x:1 });`

Object.create()是系统内置函数，参数为对象，返回新创建的对象，并且该对象的原型指向参数

创建空对象，并且对象的 __proto__ 指向参数，既继承了属性和方法，本身又有自己的空对象，对于自己添加的属性和方法不会去更改原型上的属性和方法。

# 3. call() 和 apply()
call() 和 apply() 是为了动态改变 this 而出现的，当一个 Object 没有某个方法时，
但是其他 Object 有，可以借助 call() 和 apply() 用其他对象的方法来操作。

call() 和 apply() 就是更改 this 指向
**例：**
```
        function Person(name){
            this.name = name;
        }
        function Student(name,Klass){
            Person.call(this,name); 
            //此时的 Person 的 this 指向了 Student（即 call 里面的 this）
            this.klass = klass;
        }
```

call() 和 apply() 的区别：call() 的参数是扁平化参数，apply() 的第二参数放在数组中

**例：**
```
        Math.max.apply(,)
        Math.max(1,2,3); // 3
        Math.max([1,2,3]); // NaN
        Math.apply(null,[1,2,3]); // 3
        Math.max.apply(null,[1,2,3]) === Math.max(1,2,3); // true
        Math.max.call(null,1,2,3); // 3
        Math.max.call(null,1,2,3) === Math.max(1,2,3); // true
```

# 4. 实现继承的套路：
```
        function Person(name){
            this.name = name;
        }

        Person.prototype.sayHi = fucntion(){
            console.log('hi!I am '+this.name);
        }

        function Student(name,Klass){
            Person.call(this,name);  // 使用 call 继承属性
            this.klass = klass; // 设置自身的属性
        }

        // 使用 Object.create() 继承方法
        Student.prototype = Object.create(Person.prototype,{
            constructor:{
            value:Student;
             // 还可以指定 writable configurable enumerable
            }
        });

        // 设置自身方法
        Student.prototype.learn = function(subject){
            console.log(this.name + 'can learn' + subject);
        }
```


