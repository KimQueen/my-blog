---
title: Promise
categories: ES6
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - 异步
toc: true
thumbnail: "/images/coffee.jpg"
---

## 简介

`Promise`是一种异步编程的解决方案，比传统的解决方案-- 回调行数和事件更加的合理和强大，`ES6`将其写进了语言标准，统一了用法，原生提供了`Promise`对象。而`Promise`出现主要是因为现阶段异步请求和数据之间存在依赖关系，如果使用传统的方法会产生很多难看的多层回调，这样的代码不利于进行阅读和后期的维护，俗称“回调地狱”，而且我们将错误处理代码和业务代码耦合在一起，造成代码的可读性变差，这个时候我们就引入`Promise`来降低异步编程的复杂性。

<!--more-->

那么究竟什么是`Promise`呢？简单来说`Promise`就是一个容器，里面保存了某一个未来才会结束的事件（通常是一个异步操作）的结果，在语法的意义上面，`Promise`是一个对象，从他可以获取异步操作的消息。

`Promise`的特点：

- 对象的状态是不受外界的影响的，Promise 对象代表一个异步的操作。有三个状态：`pending`，`fulfilled`和`reject`。只有异步操作的结果，可以决定当前是哪一种状态。这也是`Promise`名字的由来，`Promise`是承诺的意思，意味着其他的手段是无法改变的。
- 一旦状态发生改变就不会在进行改变了。任何时候都是这个结果，`Promise`对象的状态改变，只有两种情况`pending`变成`fulfilled`或是`pending`变成`reject`，只要状态发生改变了，状态发生改变了，状态就会凝固，不会在发生改变。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（`Event`）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

## Promise 基本语法

- `Promise`实例必须实现 then 这个方法
- `then()`必须可以接收两个函数作为参数（第二个参数是可选的）

## Promise 基础用法

`Promise`的思想是，每一个异步任务返回一个`Promise`对象，该对象有一个`then`方法，允许指定回调函数。形如这种形式：

```
f1().then(f2);
```

我们可以看到上面的写法，将回调函数变成了链式的写法，城西的流程可以很清晰的看出来，而且有一套配套的方法，这样的话可以实现很多强大的功能。
我们看一下`Promise`的实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调行数

```
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

再举一个制定多个回调函数的例子，其形式为：

```
f1().then(f2).then(f3);
```

下面我们就从一个例子进行入手，看一下上面的实现究竟是怎么样的：

```
runAsync1 = () =>{
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('异步任务1执行完成');
            resolve('随便什么数据1');
        }, 1000);
    });
    return p;
}
runAsync2 = () =>{
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('异步任务2执行完成');
            resolve('随便什么数据2');
        }, 2000);
    });
    return p;
}
runAsync3 = () =>{
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('异步任务3执行完成');
            resolve('随便什么数据3');
        }, 2000);
    });
    return p;
}

runAsync1()
.then(function(data){
    console.log(data);
    return runAsync2();
})
.then(function(data){
    console.log(data);
    return runAsync3();
})
.then(function(data){
    console.log(data);
});
```

上面的例子我们得到的结果是

![结果](https://upload-images.jianshu.io/upload_images/13681871-54ce8e2f05fa7ca9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

结果的输出：每隔两秒输出每个异步回调中的内容，在`runAsync2`中传给`resolve`的数据，能在接下来的`then`方法中拿到，那么如果我们返回的不是`Promise`对象呢，而是返回数据呢，我们将代码进行修改：

```
runAsync1()
.then(function(data){
    console.log(data);
    return runAsync2();
})
.then(function(data){
    console.log(data);
    return '直接返回数据';  //这里直接返回数据
})
.then(function(data){
    console.log(data);
});
```

打出的结果就是：

![结果](https://upload-images.jianshu.io/upload_images/13681871-0a48af162676f444.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当我们要处理有`reject`的时候，我们使用的模板是：

```
f1().then(f2(),f3());
```

上面的代码是在`f1()`的状态由`pending`变成`fulfilled`的时候执行`f2()`函数，在`pending`编程`rejected`的时候执行`f3()`函数。
我们之前一直使用 promise 来处理 ajax 的请求，我们通过代码看一下：

```
const promise = new Promise((resolve, reject) => {
  $.ajax('https://github.com/users', (value) =>  {
    resolve(value);
  }).fail((err) => {
    reject(err);
  });
});
promise.then((value) => {
  console.log(value);
},(err) => {
  console.log(err);
});

```

在上面的例子中，会在`ajax`请求成功后调用`resolve`回调函数来处理结果，如果请求失败就调用`reject`糊掉行数来处理错误，Promise 对象一般包含三个状态，分别是`pending`,`fulfilled`和`rejected`，对应`ajax`中的`pending`,`success`,`error`。成功获得值后状态就变为 fulfilled，然后将成功获取到的值存储起来，后续可以通过调用 then 方法传入的回调函数来进一步处理。而如果失败了的话，状态变为`rejected`,错误可以选择抛出（`throw`)或者调用`reject`方法来处理。
`then` 方法可以被同一个 `promise` 调用多次，`then`方法必须返回一个`promise` 对象。`

## promise 的其他用法

### promise.catch()

`promise.catch() <==>.then(null, rejection)`或`.then(undefined, rejection)`
这个方法用来指定错误发生时的回调函数，其实它和`then`的第二个参数一样，用来指定`reject`的回调

```
getNumber()
.then((data)=>{
    console.log('resolved');
    console.log(data); // 如果这里出错了，也会进入到catch函数中
})
.catch((reason)=>{
    console.log('rejected');
    console.log(reason);
});
```

效果和写在`then`的第二个参数里面一样。不过它还有另外一个作用：在执行`resolve`的回调（也就是上面`then`中的第一个参数）时，如果抛出异常了（代码出错了），那么并不会报错卡死`js`，而是会进到这个`catch`方法中

### promise.all()

其实`all`方法的效果实际上是「谁跑的慢，以谁为准执行回调」，也就是最慢的一个执行完成，才会执行回调函数。
`Promise.all`方法用于将多个`Promise`实例，包装成一个新的`Promise` 实例。这个封装的方法提供了并行执行异步操作的能力，并且所有的异步操作都执行完成后才执行回调行数。我们还是使用上面的`runAsync1`，`runAsync2`，`runAsync3`的方法测试一下：

```
Promise
.all([runAsync1(), runAsync2(), runAsync3()])
.then(function(results){
    console.log(results);
});
```

用`Promise.all`来执行，`all`接收一个数组参数，里面的值最终都算返回`Promise`对象。这样，三个异步操作的并行执行的，等到它们都执行完后才会进到`then`里面。那么，三个异步操作返回的数据哪里去了呢？都在`then`里面呢，`all`会把所有异步操作的结果放进一个数组中传给`then`，就是上面的`results`。所以上面代码的输出结果就是：

![结果](https://upload-images.jianshu.io/upload_images/13681871-8ac845b3f4f91e03.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`Promise.all接受一个promise对象的数组，待全部完成之后，统一执行success;`

### promise.race()

「谁跑的快，以谁为准执行回调」，这就是`race`方法，也就是我们如果使用了`race`的方法，一旦有执行完成的，就会执行回调行数，那我们继续举一个例子来看一下，现在我们将`runAsync1`方法的延时修改成`1s`来看一下：

```
Promise
.race([runAsync1(), runAsync2(), runAsync3()])
.then(function(results){
    console.log(results);
});
```

这个时候我们打出的结果是：

![结果](https://upload-images.jianshu.io/upload_images/13681871-013166088b2d9b6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`then`里面的回调开始执行时，`runAsync2()`和`runAsync3()`并没有停止，仍旧再执行。于是再过 1 秒后，输出了他们结束的标志。
`romise.race接受一个包含多个promise对象的数组，只要有一个完成，就执行success。`

### promise.finally()

`finally`方法用于指定不管 `Promise` 对象最后状态如何，都会执行的操作。也就是`finally`的方法是与状态无关的

```javaScript
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});  // 这个函数是一定会被执行的
```

`finally`的实现原理是：

```
Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value  => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => { throw reason })
  );
};
```

也就是返回了原来的值：

```
// resolve 的值是 undefined
Promise.resolve(2).then(() => {}, () => {})

// resolve 的值是 2
Promise.resolve(2).finally(() => {})

// reject 的值是 undefined
Promise.reject(3).then(() => {}, () => {})

// reject 的值是 3
Promise.reject(3).finally(() => {})
```

Promise 的 resolve 和 reject 和 catch（当状态码为 404,5XX 的时候会进入到 reject 中进行执行）

```
function load(){

   var xmlhttp;

   if (window.XMLHttpRequest){// code for IE7+, Firefox, Chrome, Opera,Safari

      xmlhttp= new XMLHttpRequest();

   }else{// code for IE6, IE5

      xmlhttp= new ActiveXObject("Microsoft.XMLHTTP");

   } 

   xmlhttp.onreadystatechange= function(){

      if (xmlhttp.readyState ==4 && xmlhttp.status == 200) {//获得了请求数据

 var expertinfolist = xmlhttp.responseText;

   //发送请求数据到myDiv     document.getElementById("myDiv").innerHTML=expertinfolist;

      }

   }

   var url="expert_ZFTservlet?expert_name="+"曾攀";

   xmlhttp.open("GET", '/js/jquery-ui.min.js', true);

   xmlhttp.send();

}
 a =()=>{
console.log(111)
}
b  =() =>{
console.log(222)
}
promise = new Promise((a,b)=>{
	load()
});
```

```
function load(){

   var xmlhttp;

   if (window.XMLHttpRequest){// code for IE7+, Firefox, Chrome, Opera,Safari

      xmlhttp= newXMLHttpRequest();

   }else{// code for IE6, IE5

      xmlhttp= newActiveXObject("Microsoft.XMLHTTP");

   } 

   xmlhttp.onreadystatechange= function(){

      if (xmlhttp.readyState ==4 && xmlhttp.status == 200) {//获得了请求数据

 var expertinfolist = xmlhttp.responseText;

   //发送请求数据到myDiv     document.getElementById("myDiv").innerHTML=expertinfolist;

      }

   }

   var url="expert_ZFTservlet?expert_name="+"11";

   xmlhttp.open("GET", '', true);

   xmlhttp.send();

}
const promise = new Promise((a,b)=>{
	load()
}).then(()=>{
	console.log(111)
}).catch(()=>{
console.log(222)
})

```
