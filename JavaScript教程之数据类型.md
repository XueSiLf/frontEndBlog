---
title: JavaScript教程之数据类型
date: 2018-07-06 08:57:55
catehories: JavaScript
tags: JavaScript基础
cover_picture: images/github-second.jpg
---
<!-- more -->
[TOC]

# JavaScript之数据类型

注：本笔记参考网道 JavaScript教程：http://wangdoc.com/javascript/index.html

## 概述

### 1. 简介

JavaScript语言的每一个置值，都属于某一种数据类型。JavaScript的数据类型共有6种。（ES6又新增了第7种Symbol类型的值，本教程不涉及）

* 数值（number）：整数和小数（比如1 和 3.14）。
* 字符串（string）：文本（比如Hello Word）。
* 布尔值（boolean）：表示真伪的两个特殊值，即true（真）和false（假）。
* undefined：表示”未定义“或不存在，即由于目前没有定义，所以此处暂时没有任何值。
* null：表示空值，即此处的值为空。
* 对象（object）：各种值的集合。

通常，数值、字符串、布尔值这三种类型，合称为原始类型（primitive type）的值，即它们是最基本的数据类型，不能再细分了。对象则称为合成类型（complex type）的值，因为一个对象往往是多个原始类型的值的合成，可以看作是一个存放各种值的容器。至于undefined和null，一般将它们看成两个特殊值。

对象是最复杂的数据类型，又可以分为三个子类型。

* 狭义的对象（object）
* 数组（array）
* 函数（function）

狭义的对象和数组是两种不同的数据组合方式，除非特殊声明，本教程的”对象“

### 2. typeof 运算符

JavaScript 有三种方法，可以确定一个值到底是什么类型。

* typeof 运算符
* instanceof 运算符
* Object.prototype.toString 方法

instanceof 运算符 和 Object.prototype.toString 方法，将在后文介绍。这里介绍 typeof 运算符。

typeof 运算符可以返回一个值的数据类型。

数值、字符串、布尔值分别返回 number 、string、boolean。

~~~javascript
typeof 123 // "number"
typeof '123' // "string"
typeof false // "boolean"
~~~



函数返回 function。

~~~javascript
function f() {}
typeof f // "function"
~~~



undefined 返回 undefined。

~~~javascript
typeof undefined // "undefined"
~~~

利用这一点，typeof可以用来检查一个没有声明的变量，而不把报错。

~~~javascript
v // ReferenceError: v is not undefined
typeof v // "undefined"
~~~

上面代码中，变量v 没有用var命令声明，直接使用就会报错。但是，放在typeof 后面，就不报错了，而是返回 "undefined"。

实际编程中，这个特点通常用在判断语句。

~~~javascript
// 错误的写法
if (v) {
    // ...
}
// ReferenceError: v is not defined

// 正确的写法
if (typeof v === "undefined") {
    // ...
}
~~~

对象返回 object。

~~~javascript
typeof window // "object"
typeof {} // "object"
typeof [] // "object"
~~~

上面代码中，空数组（[]）的类型也是 object，这表示在JavaScript内部，数组本质上只是一种特殊的对象。这里顺便提一下，instanceof 运算符可以区分数组和对象。instanceof 运算符的详细解释，请见《面向对象编程》一章。

~~~javascript
var o = {};
var a = [];

o instanceof Array // false
a instanceof Array // true
~~~

null 返回 object。

~~~javascript
typeof null // "object"
~~~

null 的类型是 object，这是由于历史原因造成的。1995年的JavaScript语言第一版，只设计了五种数据类型（对象、整数、浮点数、字符串和布尔值），没考虑 null，只把它当作 object 的一种特殊类型。后来 null 独立出来，作为一种单独的数据类型，为了兼容以前的代码， typeof null 返回 object 就没法改变了。



### 3. 参考链接

- Axel Rauschmayer, [Improving the JavaScript typeof operator](http://www.2ality.com/2011/11/improving-typeof.html)

## null，undefined和布尔值

### 1. null 和 undefined

#### 1.1 概述

null 与 undefined 都可以表示”没有“，含义非常相似。将一个变量赋值为undefined 或 null，老实说，语法效果几乎没区别。

~~~javascript
var a = undefined;
// 或者
var a = null;
~~~

上面代码中，变量a分别被赋值为 undefined 和 null，这两种写法的效果几乎等价。

在 if 语句中，它们都会被自动转为 false，相等运算符（==）甚至直接报告两者相等。

~~~javascript
if (!undefined) {
    console.log('undefined is false');
}
// undefined is false

if (!null) {
    console.log('null is false');
}
// null is false

undefined == null
// true
~~~

从上面代码可见，两者的行为是何等相似！谷歌公司开发的JavaScript语言的替代品Dart语言，就明确规定只有 null ，没有 undefined！

既然含义与用法都差不多，为什么要同时设置两个这样的值，这不是无端地增加复杂度，令初学者困扰吗？这与历史原因有关。

1995年 JavaScript 诞生时，最初像 Java 一样，只设置了 null 表示 “无”。根据C语言的传统，null 可以自动转为 0,。

~~~javascript
Number(null) // 0
5 + null // 5
~~~

上面的代码中，null 转为数字时，自动变成 0。

但是，JavaScript的设计者 Brendan Eich，觉得这样做还不够。首先，第一版的JavaScript 里面，null 就像 Java 里一样，被当成一个对象，Brendan Eich 觉得表示“无“的值最好不是对象。其次，那时的 JavaScript 不包括错误处理机制，Brendan Eich 觉得，如果 null 自动转为0，很不容易发现错误。

因此，他又设计了一个 undefined。区别是这样的：null 是一个表示“空”的对象，转为数值时为0；undefined 是 一个表示“此处无定义”的原始值，转为数值时为 NaN。

~~~javascript
Number(undefined) // NaN
5 + undefined // NaN
~~~

#### 1.2 用法和含义

对于 null 和 undefined，大致可以像下面这样理解。

null 表示空值，即该处的值现在是空。调用函数时，某个参数未设置任何值，这时就可以传入 null ，表示该参数为空。比如，某个函数接收引擎抛出的错误作为参数，如果运行过程中未出错，那么这个参数就会传入 null，表示未发生错误。

undefiend 表示“未定义”，下面是返回 undefined 的典型场景。

~~~javascript
// 变量声明了，但没有赋值
var i;
i // undefined

// 调用函数时，应该提供的的参数没有提供，该参数等于 undefined
function f(x) {
    return x;
}
f() // undefined

// 对象没有赋值的属性
var o = new Object();
o.p // undefined

// 函数没有返回值时，默认返回 undefined
function f() {}
f() // undefined
~~~



### 2. 布尔值

布尔值代表“真”和“假”两个状态。“真”用关键字 true 表示，“假” 用关键字 false 表示。

布尔值只有这两个值。

下列运算符会返回布尔值：

* 前置逻辑运算符：!(Not)
* 相等运算符：===，!==，===，!=
* 比较运算符：> ，>=，<，<=

如果 JavaScript 预期某个位置应该是布尔值，会将该位置上现有的值自动转为布尔值。转换规则是除了下面六个值被转为 false，其他值都视为 true。

* undefined 
* null
* false
* 0
* NaN
* ""或''（空字符串）

布尔值往往用于程序流程的控制，请看一个例子。

~~~javascript
if ('') {
    console.log('true');
}
// 没有任何输出
~~~

上面代码中，if 命令后面的判断条件，预期应该是一个布尔值，所以 JavaScript 自动将空字符串，转为布尔值 false，导致程序不会进入代码块，所以没有任何输出。

注意，空数组（[]）和空对象（{}）对应的布尔值，都是 true。

~~~javascript
if ([]) {
    console.log('true');
}
// true

if ({}) {
    console.log('true');
}
// true
~~~

更多关于数据类型转换的介绍，参见《数据类型转换》一章。

### 3. 参考链接

* Axel Rauschmayer, [Categorizing values in JavaScript](http://www.2ality.com/2013/01/categorizing-values.html)



## 数值

### 1. 概述

#### 1.1 整数和浮点数

JavaScript 内部，所有数字都是以64位浮点数形式储存，即使整数也是如此。所以，1 与 1.0 是相同的，是同一个数。

~~~javascript
1 === 1.0 // true
~~~

这就是说，JavaScript 语言的底层根本没有整数，所有数字都是小数（64位浮点数）。容易造成混淆的是，某些运算只有整数才能完成，此时JavaScript 会自动把 64位浮点数，转为32位整数，然后再进行计算，参见《运算符》一章的“位运算”部分。

由于浮点数不是精确的值，所以涉及小数的比较和运算要特别小心。

~~~javascript
0.1 + 0.2 === 0.3 // false

0.3 / 0.1 // 2.9999999999999996

(0.3 - 0.2) === (0.2 - 0.1) //false
~~~



#### 1.2 数值精度

根据国际标准IEEE 754，JavaScript浮点数的64个二进制位，从最左边开始，是这样组成的。

* 第1位：符号位，0 表示正数，1 表示负数。
* 第2位到第12位（共11位）：指数部分
* 第13位到第64位（共52位）：小数部分（即有效数字）

#### 1.3 数值范围

### 2. 数值的表示法

### 3. 数值的进制

### 4. 特殊数值

#### 4.1 正零和负零

#### 4.2 NaN

#### 4.3 Infinity

### 5.与数值相关的全局方法

#### 5.1 parseInt()

#### 5.2 parseFloat()

#### 5.3 isNaN()

#### 5.4 isFinite()

### 6. 参考链接

## 字符串

## 对象

## 函数

## 数组

