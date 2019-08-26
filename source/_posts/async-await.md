---
title: async/await原理
categories: 重学前端
tag:
  - 前端
  - 异步
toc: true
thumbnail: "/images/color.jpg"
---
我们都知道`async/await`是`Generator`函数的语法糖，为了更加深刻的了解`async/await`的原理，我们先来研究一下`Generator`的相关的知识。
<!--more-->
## 基本概念

`Generator`：可以被认为是一个状态机，其内部封装了多个内部的状态。执行该函数会返回一个遍历器对象，也就是，`Generator`函数除了状态，还是一个遍历器对象生成函数，返回的遍历器对象，可以依次遍历`Generator`内部的每一个状态。

函数特征：
- `function`关键字与函数名之间有一个星号
- 函数内部使用`yield`表达式，定义不同的内部状态

```javaScript
function* foo(){
  yield 'hello';
  yield 'word';
  return 'ending';
}
var hw = foo();
```
上面的代码定义了一个`Generator`函数，内部由两个`yield`表达式(`hello`和`world`),即将执行的函数有三个状态：`hello`,`world`和`return`语句。

`Generator`函数调用和普通函数的调用是一样的，也是在函数名后面加上一对圆括号。但是调用`Generator`调用之后并不执行，返回的也不是函数运行结果，而是指向内部状态的指针对象，也就是上面说的遍历器对象。下一步必须调用遍历对象的`next`方法，让指针指向下一个状态，也就是说，每一次调用`next`方法，内部指针就会从函数头或是上一次停下来的地方开始执行，直到遇到下一个`yield`表达式为止。换言之，`Generator` 函数是分段执行的，`yield`表达式是暂停执行的标记，而`next`方法可以恢复执行。
```javaScript
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```
`done`属性的值`false`，表示遍历还没有结束。否则表示遍历已经结束啦。

第四次调用，此时 `Generator` 函数已经运行完毕，`next`方法返回对象的`value`属性为`undefined`，`done`属性为`true`。以后再调用`next`方法，返回的都是这个值。

在这个过程中，最重要的是`next`的方法的运行逻辑，下面我们对这个过程进行一下梳理：
- 遇到`yield`表达式，先暂停后面的操作，将紧跟着`yield`后面的表达式的值，作为返回对象的`value`属性值进行返回
- 下一次调用`next`方法的时候，再继续往下执行，知道遇到下一个`yield`表达式。
- 如果没有遇到新的`yield`表达式，就一直运行直到函数结束，直到`return`语句为止，并将`return`语句后面的表达式的值，作为返回的对象的`value`属性值。
- 如果没有遇到`return`语句，就会返回对象的`value`属性值为` undefined`。

`yield`表达式与`return`语句既有相似之处，也有区别。相似之处在于，都能返回紧跟在语句后面的那个表达式的值。区别在于每次遇到`yield`，函数暂停执行，下一次再从该位置继续向后执行，而`return`语句不具备位置记忆的功能。一个函数里面，只能执行一次（或者说一个）`return`语句，但是可以执行多次（或者说多个）`yield`表达式。
### 异步操作的同步化表达
我们举一个例子来实现将异步的操作同步化：过 `Generator` 函数部署 Ajax 操作，可以用同步的方式表达。
```javaScript
function* main() {
  var result = yield request("http://some.url");
  var resp = JSON.parse(result);
    console.log(resp.value);
}

function request(url) {
  makeAjaxCall(url, function(response){
    it.next(response);
  });
}

var it = main();
it.next();
```
`makeAjaxCall`函数中的`next`方法，必须加上`response`参数，因为`yield`表达式，本身是没有值的，总是等于`undefined`。

## Generator 函数的异步应用
ES6诞生以来主要实现异步编程的方法有如下的四种：
- 回调函数 -- 回调地狱
- 事件监听
- 发布订阅
- Promise 对象 -- 代码冗余
### 回调函数
`JavaScript`语言对异步编程的实现，就是回调函数，回调函数，就是把任务的第二阶段单独写在一个函数中，等待重新执行这个任务的时候，就直接调用这个函数，回调函数的英文名字就是`callback`,直接翻译过来就是"重新调用"
```javaScript
fs.readFile('/etc/passwd', 'utf-8', function (err, data) {
  if (err) throw err;
  console.log(data);
});
```
`readFile`函数的第三个参数，就是回调函数，也就是任务的第二段。等到操作系统返回了`/etc/passwd`这个文件以后，回调函数才会执行。
### Promise 
回调函数本身并没有问题，它的问题出现在多个回调函数嵌套。假定读取`A`文件之后，再读取`B`文件，代码如下。
```javaScript
fs.readFile(fileA, 'utf-8', function (err, data) {
  fs.readFile(fileB, 'utf-8', function (err, data) {
    // ...
  });
});
```
会出现多重回调的情况，进而造成代码的可读性变差，形成`回调地狱('callback hell')`

`Promise `对象就是为了解决这个问题而提出的。它不是新的语法功能，而是一种新的写法，允许将回调函数的嵌套，改成链式调用。采用 `Promise`，连续读取多个文件，
```javaScript
var readFile = require('fs-readfile-promise');

readFile(fileA)
.then(function (data) {
  console.log(data.toString());
})
.then(function () {
  return readFile(fileB);
})
.then(function (data) {
  console.log(data.toString());
})
.catch(function (err) {
  console.log(err);
});
```
`Promise `的最大问题是代码冗余，原来的任务被` Promise `包装了一下，不管什么操作，一眼看去都是一堆`then`，原来的语义变得很不清楚。

### Generator 函数
协程有点像函数，又有点像线程。它的运行流程大致如下。
- 协程`A`开始执行。
- 协程`A`执行到一半，进入暂停，执行权转移到协程`B`。
- （一段时间后）协程B交还执行权。
- 协程`A`恢复执行。

`Generator` 函数是协程在` ES6` 的实现，最大特点就是可以交出函数的执行权，下面看看如何使用 `Generator `函数，执行一个真实的异步任务。
```javaScript
var fetch = require('node-fetch');

function* gen(){
  var url = 'https://api.github.com/users/github';
  var result = yield fetch(url);
  console.log(result.bio);
}

var g = gen();
var result = g.next();

result.value.then(function(data){
  return data.json();
}).then(function(data){
  g.next(data);
});
```
## async/await
前文有一个` Generator` 函数，依次读取两个文件。
```javaScript
const fs = require('fs');

const readFile = function (fileName) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function(error, data) {
      if (error) return reject(error);
      resolve(data);
    });
  });
};

const gen = function* () {
  const f1 = yield readFile('/etc/fstab');
  const f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```
上面代码的函数`gen`可以写成`async`函数，就是下面这样。
```javaScript
const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```
`async`函数就是将` Generator `函数的星号`（*）`替换成`async`，将`yield`替换成`await`
`async`函数对 `Generator `函数的改进
- 内置执行器。
而`async`函数自带执行器。也就是说，`async`函数的执行，与普通函数一模一样，只要一行。
- 更好的语义
`async`和`await`，比起星号和`yield`，语义更清楚了。`async`表示函数里有异步操作，`await`表示紧跟在后面的表达式需要等待结果。
- 更广的适用性。
`yield`命令后面只能是 `Thunk` 函数或 `Promise `对象，而`async`函数的`await`命令后面，可以是` Promise `对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即` resolved` 的` Promise `对象）。
- 返回值是` Promise`。
`async`函数的返回值是 `Promise` 对象
### async 函数的实现原理
`async` 函数的实现原理，就是将 `Generator` 函数和自动执行器，包装在一个函数里。
```javaScript
async function fn(args) {
  // ...
}

// 等同于

function fn(args) {
  return spawn(function* () {
    // ...
  });
}
```
所有的`async`函数都可以写成上面的第二种形式，其中的`spawn`函数就是自动执行器,我们看一下`spawn`的执行过程：
```javaScript
function spawn(genF) {
  return new Promise(function(resolve, reject) {
    const gen = genF();
    function step(nextF) {
      let next;
      try {
        next = nextF();
      } catch(e) {
        return reject(e);
      }
      if(next.done) {
        return resolve(next.value);
      }
      Promise.resolve(next.value).then(function(v) {
        step(function() { return gen.next(v); });
      }, function(e) {
        step(function() { return gen.throw(e); });
      });
    }
    step(function() { return gen.next(undefined); });
  });
}
```