---
title: redux的入门
categories: redux
tag:
  - 前端
  - react
  - redux
toc: true
thumbnail: "/images/redux.jpg"
---

## Action
`Action`本质上是`JavaScript`普通对象，我们约定，`action`内使用一个字符串类型的`type`字段表示要执行的动作。多数情况下，`type`会被定义成字符串常量。另外，当应用规模越来越大时，建议使用单独的模块或文件来存放`action`。除了`type`字段外，`action`对象的结构完全由我们自己决定。
```
store.dispatch({type:'INCREMENT'});
store.dispatch({type:'INCREMENT'});
store.dispatch({type:'DECREMENT'});
```

<!--more-->
##  Reducer
`Reducer`是个形式为`(state,action)=>state`的纯函数，描述了`action`如何把`state`转变成下一个`state`。
```
function counter(state = 0,action){
  switch(action.type){
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}

我们输出的结果是：
pre state:0 next state :1
pre state:1 next state :2
pre state:2 next state :1
```
##  纯函数
**纯函数**：输入/输出数据流都是显式的（显式：函数与外界交换数据只有一个唯一渠道--参数和返回值）
函数从函数外接受的所有输入信息都通过参数传递到该函数的内部；函数输出到函数外部的所有信息都通过返回值传递到函数外部。
纯函数不能访问外部的变量，它能接触的‘外地人’只有来自外部的参数。纯函数不能修改参数，因为这样做可能会把一些信息通过输入参数，夹带到外界。
在`reducer`函数中不能进行如下的参数：
- 修改传入的参数
- 执行有副作用的操作，如：`API`请求和路由跳转
- 调用非纯函数，如`Date.now()`或`Math.random()`
##  不能修改参数state
假如我们尝试一下修改`state`并返回，如下面的代码：
```
Counter = (state = 0,action) => {
  switch (action.type){
    case 'INCREMENT':
      state++;
      return state;
    case 'INCREMENT':
      state--;
      return state;
    default:
      return state;
  }
}
```
运行后发现，并没有报错，打印的结果和之前一样，这样的话，我们就会考虑是不是我们的规则有问题，而其实这种情况只是一种侥幸。
当`state`是对象的时候，修改参数`state`会影响程序的变更追踪。如果后面使用了`react-redux`将`Redux`程序连接到`UI`组件上，你的组件将不能更新，因为`react-redux`无法察觉到任何`state`变化。
为什么会发生这样的问题？因为在`JavaScript`中，对象是引用类型，当我们改变`state`，变化前后的两个`state`就会指向一个地址，`react-redux`就会以为这是两个相同的`state`，因而不会执行渲染。
让我们将`state`给为对象类型，我们看一下这个违规的行为带来的后果：
```
Counter = (state ={val:0},action) => {
  switch (action.type){
    case 'INCREMENT':
      state.val++;
      return state;
    case 'INCREMENT':
      state.val--;
      return state;
    default:
      return state;
  }
}
```
这个时候我们再一次对state的值进行打印，我们发现结果发生了改变：
`pre state:{val:1} next state :{val:1} `
`pre state:{val:2}  next state :{val:2} `
`pre state:{val:1}  next state :{val:1}`
## Store
`Store`是一个全局的对象，将`action`和`reduce`以及`state`联系到了一起。`store`有以下的职能：
- 维持应用的`state`
- 提供`getState()`方法获取`state`
- 提供`dispatch(action)`方法更新`state`
- 通过`subscribe(listener)`注册监听器 

##  创建
创建`store`需要从`redux`包中导入`createStore`这个方法：
```
import {createStore} from 'redux'
```
使用`reducer`纯函数作为第一个参数创建`store`：
```
let store = createStore(Counter);
```
这个例子中只有一个参数，但是可以将初始`state`作为第二个参数传入。修改`app.js`中的`createStore`函数，添加第二个参数100作为初始`state`。
```
let store = createStore(Counter,100);
```
我们输出的结果是：
`pre state:100 next state :101`
`pre state:101 next state :102`
`pre state:102 next state :101`
## 获取与监听
在上面我们创建完`store`，使用它获取数据，并监听变化：
```
const store = createStore(Counter);
let currentValue = store.getState(); 
store.subscribe(()=>{
    const previousValue = currentValue;
    currentValue = store.getState();
    console.log('pre state:', previousValue, 'next state:' ,currentValue );
  }
);
```
上面的代码做了这样的几件事：
- 获取初始化的`state`到`currentValue`。
- 使用`store.subscribe()`方法监听变化
- 在`store.subscribe()`的回调函数中，将`currentValue `传给`previousValue `作为先前的`state`
- 获取更新后的`state`到`currentValue `当做当前的`state`
- 打印变化前后的`state`

所以我们的输出的结果就是：
`pre state:0 next state :1`
`pre state:1 next state :2`
`pre state:2 next state :1`
##  发起action
对于`store`来说使用`dispatch(action)`方法发起`action`，更新`state`：
```
store.dispatch({type:'INCREMENT'});
store.dispatch({type:'INCREMENT'});
store.dispatch({type:'DECREMENT'});
```
当发起`action`后，就将`action`传进了`store`中，使用`reducer`纯函数执行更新。改变内部`state`唯一方法是`dispatch`一个`action`。这样确保了视图和网络请求都不能直接修改`state`，相反它们只能表达想要修改的意愿，也就是`dispatch`一个`action`。
上面的代码我们写一下完整版的代码：
```
import {createStore} from 'redux';

Counter = (state ={val:0},action) => {
  switch (action.type){
    case 'INCREMENT':
      state.val++;
      return state;
    case 'INCREMENT':
      state.val--;
      return state;
    default:
      return state;
  }
}

let currentValue = store.getState(); 
const listener = ()=>{
    const previousValue = currentValue;
    currentValue = store.getState();
    console.log('pre state:', previousValue, 'next state:' ,currentValue );
  }

store.subscribe(listener);

store.dispatch({type:'INCREMENT'});
store.dispatch({type:'INCREMENT'});
store.dispatch({type:'DECREMENT'});
```