---
title: js：作用域
Date: 2020-05-14
tags: [JS]
categories: JS
comments: true
---


作用域就是变量和函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期。

### 全局作用域
在代码任何地方都能访问到的对象拥有全局作用域。   
拥有作用域的情况：
- 程序最外层定义的函数或变量。
- 所有未定义直接赋值的变量，相当于在window对象上创建属性。（不推荐）
- 所有window对象的属性和方法。

### 局部作用域（函数作用域）
局部作用域在函数内创建，在函数内可访问，函数外不可访问。

### 变量声明
js引擎解析js代码时，会先把变量和函数的声明提前进行预解析，然后再去执行其他代码。

直接赋值不会引起变量提升，在执行期间创建，为window的属性，可删除。

### 函数声明
1. function name(){}直接创建
2. new Function构建函数创建
3. 给变量赋值匿名函数方法创建
```
var name = function(){ }
```
后两者，在声明前访问，返回undefined。  
函数名与变量名声明时相同，函数优先声明。

### 作用域链
- 当代码在一个环境中执行时，一般情况下，会到当前执行环境中访问变量。
- 但是如果在当前作用域中没有查到值，就会向上级作用域去查，直到查到全局作用域，这么一个查找过程形成的链条（即这样逐层的作用域形成的链条）就叫做作用域链。
- 作用域链是函数被创建的作用域中对象的集合。作用域链保证了对变量和函数的有序访问。
- 每次进入一个新的执行环境，都会创建一个用于搜索变量和函数的作用域链。

作用域链的前端，始终是当前执行代码所在环境的变量对象。如果当前环境是函数，则将其活动对象作为变量对象。活动对象最开始只包含arguments对象（这个对象在全局环境中不存在）。

全局执行环境的变量对象始终都是作用域链的最后一个对象。

js每一个函数执行时，会先在自己创建的AO上找对应的属性值。若找不到，则往父函数的AO上找，直到找到全局作用域，这样就形成一条作用域链。

VO（变量对象）：函数创建阶段，js解析引擎进行预解析时，所有变量和函数的声明组成VO。

AO（活动对象）：函数执行阶段，当函数被调用执行时，会建立一个执行上下文，该执行上下文包含了函数所需的所有变量，这些变量组成AO。

执行上下文可以理解为当前代码的执行环境，它会形成一个作用域。
