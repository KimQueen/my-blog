---
title: JavaScript的类型详解
categories: 重学前端
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tags:
  - 前端
  - 基础
  - JavaScript
thumbnail: "/images/window.jpg"
---

## JavaScript 的类型

javaScript 语言定义了七种语言类型，我们看一下都有哪些？

<!--more-->

- Undefined
- null
- Boolean
- String
- Number
- Symbol (ES6 新添加)
- Object

### undefined 和 null

undefined：表示类型未定义
null：表示定义了，但是值为空

### Boolean

表示逻辑上的真和假，这个类型只有两个值：true 和 false

### String

String 表示文本数据，String 的最大是 2^53 - 1，String 的意义并非“字符串”，而是字符串的 UTF16 编码

### Number

其中有一个特殊的值`NaN`，这个值是一个非自反的值：

```JavaScript
NaN === NaN // flase
NaN == NaN // flase
```

- NaN:占用了 9007199254740990，这原本是符合 IEEE 规则的数字；
- Infinity:无穷大
- -Infinity:无穷小

```JavaScript
console.log(0.1+0.2 == 0.3); // false
```

这两边是不相等的，因为根据浮点数的特性来决定,这个是因为浮点数计算的精度问题。
解决的方法：正确的比较方法是使用 JavaScript 提供的最小精度值

```JavaScript
console.log(Math.abs(0.1+0.2-0.3) <= Number.EPSILON);
```

### Symbol

Symbol 是 ES6 中引入的新类型，他是一切非字符串的对象 key 的集合，在 ES6 规范中，整个对象系统被用 Symbol 重塑
创建 Symbol 的方式是使用全局 Symbol 函数：

```JavaScript
var mySymbol =Symbol("my school")
```

### Object

Object 表示对象的意思，是一切的有形和无形的物体的总称。
在 JavaScript 中，对对象的定义是`属性的集合`,属性分为：数据属性和访问器属性。
对于 JavaScript 来说，有几个基本类型都有一个对应的对象类型：

- Number
- String
- Boolean
- Symbol

也就是说 Number(3)和 3 并不是一个意思，一个是对象类型一个是 Number 类型。
其中 Number、String、Boolean 这三个构造器是两用的，在和 new 搭配的时候，会产生对象，当直接调用的时候，可以进行强制类型转换。
Symbol 函数就会比较特殊，直接使用 new 的话，会抛出错误，但是它仍然是 Symbol 对象的构造器。

其中数组时一种特殊的对象：

```JavaScript
var a = [];
a[0] = 1;
a['kim'] = 'jin';

console.log(a.length) // 1
console.log(a['kim']) // jin
console.log(a.kim) // jin
console.log(a) // [1, kim: "jin"]
```

我们注意一下数组通过数字进行索引，但是他们在实际的意义上也是对象，所以也可以包含字符串键值和属性（但是这些并不在长度的计算范围内）

### 值和引用

在 JavaScript 中，对值的和引用赋值或是传递在语法上是没有区别的，主要是靠类型来进行决定的

```JavaScript
var a = 2;
var b = a ; // b是a的值的一个副本
b++;
a;// 2
b; // 3

var c = [1,2,3];
var d = c; // d 是[1,2,3.]的一个引用
d.push(4);
c; // [1,2,3,4]
d; // [1,2,3,4]
```

也就是简单值(基本的类型值),总是通过复制的方式来进行赋值/传递的。包括 null、undefined、字符串、数字、布尔、ES6 中的 symbol。

复合值-- 对象和函数，通过引用复制的方式来赋值/传递

```JavaScript
var a = [1,2,3];
var b = a;
a; // [1,2,3]
b; // [1,2,3]

b = [4,5,6];
a; // [1,2,3]
b; // [4,5,6]
```

我们通过下面的代码演示一下：

```JavaScript
 function foo(x){
     x.push(4);
     x; // [1,2,3,4]

     x= [4,5,6];
     x.push(7);
     x; // [4,5,6,7]
 }

var a = [1,2,3];
a; // [1,2,3,4]
```

上面的代码是向函数传递 a 的时候，实际就是将引用 a 的提个副本赋值给 x，而 a 的指向还是指向[1,2,3], `x= [4,5,6]`这一行代码并不会一想 a 的指向，所以 a 依旧指向`[1,2,3,4]`。
如果我们如果想要上面的代码打出的`[4,5,6,7]`的话，我们可以将代码修改为如下的方式：

```JavaScript
function foo (x){
    x.push = 4;
    x; // [1,2,3,4]

    x.length = 0; // 清空数组
    x.push(4,5,6,7);
    x; // [4,5,6,7]
}

var a = [1,2,3];
a; // [4,5,6,7]                                                                                  
```

### 类型转换

基本的类型转换如下表：

```javaScript
Boolean(Null) === False
Boolean(Undefined) === False
Boolean(0) === false // 数字除了0和NaN都为true
Boolean(NaN) === false
Boolean('') === false // 字符串除了‘’都为true
Boolean(Symbol(11))=== true
Boolean(Object()) === true

Number(Null) === 0
Number(undefined) === NaN
Number(Boolean(true)) === 1
Number(Boolean(false)) === 0
Number(Symbol('11')) // TypeError

String(null) === 'null';
String(undefined) === 'undefined';
String(Boolean(true)) === TRUE;
String(Boolean(false)) === FALSE;
String(Symbol('11')) // TypeError

Object(Null) // TypeError
Object(undefined) // TypeError
```

上面的都是一些基本的，下面还有一些比较特殊的，我们进行详细的阐述

#### stringToNumber

字符串到数字的类型转换，存储在一个语法结构，类型转换支持十进制、二进制和十六进制，如：

- 30
- 0b111
- 0o13
- 0XFF

但是 ParseInt 和 ParseFloat 使用的并不是这个转换，在不传入第二个参数的情况下 ParseInt 只支持 16 进制的前缀"0x"而且会忽略非数值的字符串，也不支持科学计数法，大部分的情况下 Number 是比 ParseInt 和 ParseFloat 更好的选择。

```JavaScript
console.log(parseInt('hhhj8889')); // NaN
console.log(parseInt('8889hhhj')); // 8889
console.log(parseInt('8hhhj889')); // 8
```

#### NumberToString

在比较小的范围内，数字到字符串的转换完全就是直接的转换，但是当 Number 绝对值比较大或者是较小的情况下字符串的标识就会直接使用科学计数法表示。

#### 装箱转换

什么是装箱转换：就是将基本的数据类型转换成对象的对象，它是类型转换中一种相当重要的种类,但是转向机制会产生临时对象，在一些对性能比较高的场景下，我们应该尽量避免基本类型的装箱转换。

call本身会产生装箱操作，所以需要配合 typeof 来区分基本类型还是对象类型。

#### 拆箱转换

拆箱转换：对象类型到基本类型的转换（拆箱转换）
对象到String和Number的转换都遵循"先拆箱在转换"的规则，通过拆箱转换，吧对象编程基本类型，再从基本类型转换成对象的String或是Number。
对于拆箱会尝试调用valueOf和toString来获取拆箱后的基本类型，如果valueOf和toString都不存在，或是没有返回基本类型，就会出现TYpeError的错误。
```JavaScript
var o = {
    valueof:()=>{
        console.log("valueOf");
        return ();
    }
    toString:()=>{
        console.log("toString");
        return();
    }
}

o*2
// valueOf
// toString
// TypeError
```
我们定义了一个对象o，o有valueOf和toString两个方法，这两个方法都返回一个对象，然后我们进行o*2这
个运算的时候，你会看见先执行了valueOf，接下来是toString，最后抛出了一个TypeError，这就说明了这
个拆箱转换失败了。
但是到了String的拆箱转换会有限调用toString。我们把刚刚的运算从o*2换成String，我们发现调用的顺序发生了改变：
```JavaScript
var o = {
    valueof:()=>{
        console.log("valueOf");
        return ();
    }
    toString:()=>{
        console.log("toString");
        return();
    }
}

String(o)
// toString
// valueOf
// TypeErro
```