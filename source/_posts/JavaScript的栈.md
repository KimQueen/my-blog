---
title:  JavaScript的栈
categories: 数据结构
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - JavaScript
  - 栈
  - 数据结构
toc: true
thumbnail: "/images/name.jpg"
---
## 栈数据结构
栈是一种遵从后进先出（`LIFO`）原则的有序集合。新增加的或删除的元素都保存在栈的同一端，称作栈顶，另外的一端就是栈底。在栈里面，新元素都靠近栈顶，旧元素都接近栈底。
<!--more-->
#### 创建栈
我们将创建一个类来标识栈。让我们从基础开始，先声明这个类：
```
function Stack(){
  //各种属性和方法的声明
}
```
首先，我们需要一种数据结构来保存栈的元素，可以选择数组：
```
let items = [];
```
接下来，我们要为我们的栈声明一些方法。
- `push(element(s))`:添加一个（或几个）新元素到栈顶。
- `pop()`:移除栈顶的元素，同时返回被移除的元素。
- `peek()`:返回栈顶的元素，不对栈做任何的修改（这个方法不会移除栈顶的元素，仅仅是返回它）
- `isEmpty()`:如果栈里没有任何元素都返回`true`，否则返回`false`。
- `clear()`:移除栈里的所有元素
- `size()`:返回栈里的元素个数，这个方法和数组的`length`属性很类似。
#### 向栈添加元素
我们要先实现第一个方法`push`，这个方法负责往栈里添加新元素，有一点很重要：该方法只添加元素到栈顶，也就是栈的末尾。`push`方法可以这样写：
```
this.push = function (element){
  item.push(element);
}
```
因为我们使用了数组还是保存栈里的元素，所以可以用上一章学到了`push`方法来实现。
#### 从栈移除元素
接下来，我们来实现`pop`方法。这个方法只要用来移除栈中的元素。栈遵从`LIFO`原则，因此移出的是最后添加进去的元素。因此，我们可以用上一章讲数组时介绍的`pop`方法。栈的`pop`方法可以这样写：
```
this.pop = function(){
  return items.pop();
}
```
只能用`push`和`pop`方法添加和删除栈中的元素，这样一来，我们的栈自然就遵从了`LIFO`原则。
#### 查看栈顶元素
现在我们的类实现一些额外的辅助方法，如果想知道栈里最后添加的元素是什么，可以用`peek`方法，这个方法将返回栈顶的元素：
```
this.peek = function(){
  return items[items.length - 1];
}
```
![栈中的元素结构](https://upload-images.jianshu.io/upload_images/13681871-d2045830ef17d5e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如上图中，有一个包含三个元素的栈，因此内部数组的长度就是3.数组中最后一项的位数是`2`，`length-1` `（3-1）`正是`2`。
#### 检测栈是否为空
下一步我们来实现`isEmpty`，如果栈为空的话讲返回`true`，否则就返回`false`：
```
this.isEmpty = function (){
  return items.length == 0;
}
```
使用`isEmpty`方法，我们能简单的判断内部数组的长度是否为`0`.
那么下面我们来看一下`size`方法：
```
this.size = function (){
  return items.length;
}
```
#### 清空和打印栈元素
最后，我们来实现`clear`方法。`clear`方法用来移除栈中的所有元素，把栈清空。实现最简单的方式是：
```
this.clear = function (){
  items = [];
}
```
另外我们可以多调用`pop`方法，把数组中的元素全部移除，这样也能实现`clear`方法。
完成了！栈已经实现，通过一个例子来放松一下：为了检查栈里的内容，我们开实现一个辅助方法，交`print`。他会把栈里的元素输出到控制台：
```
this.print = function (){
  console.log(items.toString());
}
```
####  使用Stack类
在深入了解栈的应用前，我们先来学习如何使用`Stack`类。
首先，我们需要初始化`Stack`类。然后，验证一下栈是否为空（输出`true`，因为还没有往栈里添加元素）
```
let stack = new Stack();
console.log(stack.isEmpty()); // 输出为true

// 向栈中添加元素
stack.push(5);
stack.push(8);

//查看栈顶元素
console.log(stack.peek()); //输出 8

//再添加一个元素
stack.push(11);
console.log(stack.size()); // 输出 3
console.log(stack.isEmpty()); //输出false

//再添加一个元素
stack.push(15);
```
现在我们直观的看一下我们对栈的操作，以及栈的当前状态：

![具体操作步骤](https://upload-images.jianshu.io/upload_images/13681871-65e73b30011ceeef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后调用两次`pop`函数来移除2个元素：
```
stack.pop();
stack.pop();
console.log(stack.size()); // 输出2
stack.print(); // 输出[5,8]
```
现在我们直观的看一下我们对栈的操作，以及栈的当前状态：

![具体操作步骤](https://upload-images.jianshu.io/upload_images/13681871-64a9b48e1278f73c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## ECMAScript 6 和stack类
我们花时间分析一下代码，看看是都能用`ECMAScript6（ES6）`的新功能来改进。
我们创建一个可以当做类来使用`Stack`函数。`JavaScript`函数都有构造函数，可以用来模拟类的行为。我们生命了一个私有的`items`变量，它只能被`Stack`函数/类访问。然而，这个方法为每个类的实例都创建一个`items`变量的副本。因此，如果要创建多个`Stack`实例，它就不太适合了。
#### 用ES6 语法声明Stack类
我们看一下下面的代码：
```
class Stack {
  constructor(){
    this.item = []; //{1}
  }
  push(element){
    this.items.push(element);
  }
  //其他方法
}
```
我们只是用`ES6`的简化语法那`Stack`函数转化成`Stack`类。这种方法不能像其他语言（`Java`，`C++`，`C#`）一样直接在类里面声明变量，只能在类的构造函数`constructor`里声明在类的其他函数里用`this.nameofVariable`就可任意引用这个变量。
尽管代码看起来更简洁，更漂亮，变量`items`却是公共的。`ES6`的类是基于原型的。虽然基本原型的类比基本函数的类更节省内存，也更适合创造多个实例，却不能够声明私有属性（变量）或方法。而且，在这个情况下，我们希望`Stack`类的用户只能访问暴露给类的方法。否则，就有可能从栈的中间移除元素，这不是我们希望看到的。
看一下`ES6`的限定作用域`Symbol`实现类
1.用`ES6`的限定作用域`Symbol`实现类
`ES6`增加了一种叫做`symbol`的基本类型，它是不可变的，可以用作对象的属性，看看怎么用它来在`Stack`类中声明`items`属性：
```
let _item = Symbol(); //(1)
class Stack {
  constructor (){
    this[_items] = []; // (2)
  }
  // Stack 方法
}
```
在上面的代码中，我们声明了`Symbol`类型的变量`_items`（行1），在类的`constructor`函数中初始化它的值（行2）。要访问`_items`,只需把所有的`this.items`都换成`this[_items]`。
这种方法创建了一个私有属性，因为`ES6`新增的`Object.getOwnPropertySymbol`方法能够取到类里面声明的所有`Symbol`属性，下面是一个破坏`Stack`类的例子：
```
let stack = new Stack();
stack.push(5);
stack.push(8);
let objectSymbols = Object.getOwnPropertySymbols(stack);
console.log(objectSymbols.length); //1
console.log(objectSymbols); //[Symbol()]
console.log(objectSymbols[0]); //Symbol()
console.log(objectSymbols[0].push(1));
stack.print(); // 输出：5,8,1
```
上面的代码我们可以看到，访问`stack[objectSymbols[0]]`是可以得到`_items.`并且，`_items`属性是一个数组，可以进行任何的数组操作，比如从中间删除或是添加元素。我们操作的是栈，不应该出现这样的操作。
2.`ES6`是的`weakMap`
有一种数据类型可以确保属性是私有的，这就是`WeakMap`。我们会在以后深入探讨`Map`这种数据结构，现在只需要知道`weakMap`可以用来存储键值对，其中键是对象，值可以是任意数据类型。
如果用`weakMap`来存储`items`变量，`stack`就是这样的：
```
const items = new weakMap(); //声明一个weakMap类型的变量items
class Stack{
  constructor(){
    items.set(this, []); //以this为键，把代表栈的数组存入items
  }
  push(element){
    let s = items.get(this); //从weekMap中取出值
    s.push(element)
  }
  pop(){
    let s = items.get(this);
    let r = s.pop();
    return r;
  }
}
```
在上面的代码中，`items`在`stack`类是真正的私有属性，但是还有一件事要去做。`items`现在仍然是在`Stack`类以外声明的，因此谁都可以来修改`Stack`这个类，这个时候我们需要一个闭包(外层函数)把`Stack`类包起来没这样就只能在这个函数里访问`weakMap()`.
```
let Stack = (function(){
  const items = new weakMap();
  class Stack{
    constructor(){
      items.set(this, []);
    }
    //其他方法
    return Stack;
})（）;
```
## 用栈解决的问题
#### 从十进制到二进制
在现实生活中，我们主要使用十进制，但是在科学计算中，二进制是十分重要的，因为计算机中的所有的内容都是用二进制来表现的（0和1），没有十进制和二进制的相互转换能力，与计算机的交流就会十分的困难。要把十进制转换成二进制，我们可以将改十进制数字和2整除（二进制就是满二进一），知道结果为0为止，现在我们举个栗子，把十进制数字10转换成二进制的数组，大概的过程就是这样的：

![十进制变二进制的操作步骤](https://upload-images.jianshu.io/upload_images/13681871-c48be710ea9003e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

大学的计算机课程一般都会先教这个进制转换。下面对应的算法描述：
```
function divideby2(decNumber){
  let remStack = new Stack(),rem,binaryString = '';
  while (decNumber > 0){
    rem = Math.floor(decNumber % 2);
    remStack.push(rem);
    decNumber = Math.floor(decNumber / 2);
  }
  while(!remStack.isEmpty()){
    binaryString  += remStack.pop().toString();
  }
  return binaryString ;
}
```
从上面的算法中，我们很容易进行修改，让十进制可以转换成任意进制，除了让十进制和2整除转换为二进制，还可以传入其他任意进制的基数为参数，就像下面的算法是这样的：
```
function baseConverter(decNumber , base){
  let remStack = new Stack(),
                 rem,
                 binaryString = '';
                 digits = '0123456789ABCDEF';
  while (decNumber > 0){
    rem = Math.floor(decNumber % base);
    remStack.push(rem);
    decNumber = Math.floor(decNumber / base);
  }
  while(!remStack.isEmpty()){
    binaryString  += remStack.pop().toString();
  }
  return binaryString ;
}
```