---
title: 原生函数
categories: 重学前端
date: 2019-07-11 14:42:17
tags:
  - 前端
  - 基础
  - JavaScript
thumbnail: "/images/tree.jpg"
---
原生函数：
- String()
- Number()
- Boolean()
- Array()
- Object()
- Function()
- RegExp()
- Date()
- Error()
- Symbol() -- 在ES6中新加入的
<!--more-->

```JavaScript
var s = new String("hello word");
console.log(s.toString()); // hello word
```
一般情况下我们是不直接使用封装对象，最好的办法是让JavaScript引擎自己决定什么时候使用封装对象，也就是说我们首先考虑的是'abc'和42这样的基本类型，而不是new String("abc")和new Number(42)

## 原生函数作为构造函数的时候

对于数组(array)、对象(object)、函数(function)和正则表达式，我们通常喜欢以字面量的形式来创建它们。实际上，使用字面量和使用构造函数的效果是一样的(创建的值都是通过封装对象来包装的)

### Array(..)

```javaScript
var a = new Array(1,2,3);
a; // [1,2,3]
var  b = [1,2,3];
b; // [1,2,3]
```
 ps:Array(..)和new Array的效果是一样的，如果不带有new，它会进行自动的补全。也就是说Array(1,2,3)和new Array(1,2,3)的效果是一样的。
 
 永远都不要使用空单元数组。

 ### Object(..) Function(..) 和RegExp(..)

 和上面的数组一样，如果不是万不得已，尽量不要使用Object(..)/Function(..)/RegExp(..)

其中 new Object()来创建对象，因为这样就没有办法像使用常量的形式那样一次设定了多个属性，而是必须要逐一设定。

构造函数Function只有在极少数的情况下很有用，比如动态定义函数参数和函数体的时候，如果不是将Function(..)当做eval(..)的替代品，基本上是不会通过这样的方式来定义函数的。

建议使用常量的形式(如 /^a*b+/9)来定义正则表达式，这样的话，不仅语法简单，而且执行的效率也会更加的高，因为JavaScript引擎在代码执行前会对它们进行预编译和缓存，与前面的构造函数不同，RegExp(..)有时候还是很有用的，比如说动态的定义正则表达式的时候：
```javaScript
 var name = 'kim'
 var namePattern = new RegExp("\\b(?:"+name+")+\\b",'ig');

 var match = someText.match(namePattern);
```
### Date(..) 和Error（..）

相对于其他的原生的构造函数，Date(..)和Error(..)的用处要大的多，因为没有对应的常量作为它们的替代。
创建日期对象必须要使用new Date()。Date(..)可以带有参数，用来指定日期和事件，但是不带参数的话，就要使用当前的日期和事件。

构造函数Error(..)带还是不带new关键字都是可以的。

创建错误对象(error object) 主要是为了获取当前运行栈的上下文，栈上下文信息包括函数调用栈信息和产生错误的代码行号，为了方便调试。具体的使用就不详细阐述了。

### Symbol(..)
我们可以使用Symbol(..)原生构造函数来自定义符号，但是它比较特殊，是不能带有new的关键字的，否则的话，就会报错。

### 原生原型

原生的构造函数都有自己的.prototype对象。这些对象包含其对子类型所特有的行为特征。

在相关文档中，我们约定将String.prototype,xyz可以简写成String#xyz,对其他的.prototype也是一样的。




