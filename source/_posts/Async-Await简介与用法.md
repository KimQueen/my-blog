---
title: Async/Await简介与用法
categories: 前端异步
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - 异步
toc: true
thumbnail: "/images/key.jpg"
---
## 简介
- async/await是写异步代码的新方式，以前的方法有回调函数和Promise。
- async/await是基于Promise实现的，它不能用于普通的回调函数。
- async/await与Promise一样，是非阻塞的。
- async/await使得异步代码看起来像同步代码，这正是它的魔力所在。
<!--more-->

##  语法
 我们看一下怎么使用promise和async和await来显示两段代码：
### Promise
```
const makeRequest = () =>{
  getJSON().then(data=>{
    console.log(data);
    return 'done';
  })
}

makeRequest()
```
### async/await
```
const makeRequest = async () =>{
  console.log(await getJson())
  return "done";
}

makeRequest()
```

 我们了解到async/await是根据Promise来进行封装的，现在我们现在看一下上面的两段代码的异同：
-   函数的内部多了一个async的关键字，await 关键字只能用在async定义的函数的内部。async函数会隐式的返回一个Promise，这个Promise的resolve值就是return的值（上面的例子中resolve值就是字符串“done”）
- 我们在看语法的时候发现我们不能再最外层的代码使用await，因为不在async函数中。

 await getJSON()表示console.log会等到getJSON的promise成功reosolve之后再执行。
 对于async函数，返回的Promise对象，async函数内部return语句返回值的，会成为then方法回调函数的参数。
```
async function f(){
  return 'hello word';
}

f().then(v=>{
  console.log(v);// "hello word"
})
```

async函数返回的Promise对象，必须等到内部所有的await命令后面的Promise对象执行完，才会发生变化，除非遇到return语句或是抛出错误，也就是说，只用async函数内部的异步操作都执行完成了，才会执行then方法指向的回调函数。
对于async函数的内部函数的执行顺序：

![代码的执行顺序](https://upload-images.jianshu.io/upload_images/13681871-ac65d0611492f29d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中对于async函数来说，其中任何一个await语句后面的 Promise 对象变为reject状态，那么整个async函数都会中断执行。
  ```
async function f() {
    await Promise.reject('出错了');
    await Promise.resolve('hello world'); // 不会执行
}
```
上面代码中，第二个await语句是不会执行的，因为第一个await语句状态变成了reject。做个是async函数的特性，但是有的时候我们期望前一个异步操作失败，也不要中断第二个异步的操作，这样的话我们可以在第一个wait放在try ...catch结构中，这样的话不管这个异步操作是否成功，第二个await都会执行。
```
async function f() {
  try {
    await Promise.reject('出错了');
  } catch(e) {
  }
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// hello world
```
第二种方法就是对第一个await再一次添加一个catch方法，处理前面的错误：
```
async function f() {
  await Promise.reject('出错了')
    .catch(e => console.log(e));
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// 出错了
// hello world
```
##  对错误进行处理
1.Async/Await让try/catch可以同时处理同步和异步错误。在下面的promise示例中，try/catch不能处理JSON.parse的错误，因为它在Promise中。我们需要使用.catch，这样错误处理代码非常冗余。并且，在我们的实际生产代码会更加复杂。
```
const makeRequest = () => {
  try {
    getJSON()
      .then(result => {
        // JSON.parse可能会出错
        const data = JSON.parse(result)
        console.log(data)
      })
      // 取消注释，处理异步代码的错误
      // .catch((err) => {
      //   console.log(err)
      // })
  } catch (err) {
    console.log(err)
  }
}

```
使用aync/await的话，catch能处理JSON.parse错误
```
const makeRequest = async () => {
  try {
    // this parse may fail
    const data = JSON.parse(await getJSON())
    console.log(data)
  } catch (err) {
    console.log(err)
  }
}
```
##  对于条件语句的可读性增强
```
const makeRequest = () => {
  return getJSON()
    .then(data => {
      if (data.needsAnotherRequest) {
        return makeAnotherRequest(data)
          .then(moreData => {
            console.log(moreData)
            return moreData
          })
      } else {
        console.log(data)
        return data
      }
    })
}

```
使用async/await编写可以大大地提高可读性:
```
const makeRequest = async () => {
  const data = await getJSON()
  if (data.needsAnotherRequest) {
    const moreData = await makeAnotherRequest(data);
    console.log(moreData)
    return moreData
  } else {
    console.log(data)
    return data    
  }
}
```
##  中间值
我们现在有一个需求是这样的，调用promise1返回结果去调用promise2，然后再使用二者的结果去调用promise3.这样的话 我们的代码可能是这样的：
```
const makeRequest = () => {
  return promise1()
    .then(value1 => {
      return promise2(value1)
        .then(value2 => {        
          return promise3(value1, value2)
        })
    })
}
```
如果promise3不需要value1，可以很简单地将promise嵌套铺平。如果你忍受不了嵌套，你可以将value 1 & 2 放进Promise.all来避免深层嵌套
```
const makeRequest = () => {
  return promise1()
    .then(value1 => {
      return Promise.all([value1, promise2(value1)])
    })
    .then(([value1, value2]) => {      
      return promise3(value1, value2)
    })
}
```
但是这样的做法是为了可读性牺牲了语义，处理避免嵌套，没有任何必要将value1和value2放在一个数组里面
使用async和await进行编写的话
```
const makeRequest = async () => {
  const value1 = await promise1()
  const value2 = await promise2(value1)
  return promise3(value1, value2)
}

```
## 错误栈
函数a内部运行了一个异步任务b()。当b()运行的时候，函数a()不会中断，而是继续执行。等到b()运行结束，可能a()早就运行结束了，b()所在的上下文环境已经消失了。如果b()或c()报错，错误堆栈将不包括a()
```
const a = () => {
  b().then(() => c());
};
```
现在将这个例子改成async函数。
```
const a = async () => {
  await b();
  c();
};
```
b()运行的时候，a()是暂停执行，上下文环境都保存着。一旦b()或c()报错，错误堆栈将包括a()。