---
title: React组件中的constructor概述
categories: react
tag:
  - 前端
  - 语言
  - react
toc: true
thumbnail: "/images/brain.jpg"
---
在`react`官网中，我们看到经常会有这样声明组件的，但是我们自己在写的时候很少会使用`constructor`，`super`，那么这两个方法到底是用来做什么的呢？在写`react`中一定要写吗？我们在下面具体看一下：

<!--more-->
```javaScript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
上面的代码是在官网粘贴下来的，但是在操作的时候，我们发现们不写`constructor`，`super`也是没有问题的，并不会影响我们的执行。为什么我们要是有构造函数，后来翻阅了一下原型链的内容在`es5`中，是没有类的概念的，所以都要在原型链上实现，我们看一下下面的代码：
```javaScript
  constructor(props) {
    super(props);
  }
```
相当于对类进行一个`new`的操作，我们真正在引用的时候，使用的是这个类的实例。而我们现在在写的时候，不写上面的部分，并不是我们在运行中不再运行了，而是不管我们写不写`constructor`，在`new`实例的时候，都会为我们补上`constructor`。
那`ES5`和`ES6`在`constructor`上面扮演的角色是否是相同的呢？
- ` ES5`的继承，实质上是先创造子类的实例对象`this`,然后再将父类的方法添加到`this`上面去`(Parent.apply(this))`
- ` ES6`的继承机制：先创造父类的实例对象`this`,(必须先调用`super`方法)，然后再用子类的构造函数修改`this`。


既然讲到了这里，我们很好奇，在之前我们写的时候是需要对相对应的函数进行绑定的，那我们什么时候需要绑定，什么时候是不需要绑定的呢？
```javaScript
class Clock extends React.Component {
  state = {date: new Date()};
  onChange= () =>{
    this.setState({data:111})
  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
        <Button onClick = {this.onChange()}>确认</Button>
      </div>
    );
  }
}
```
上面的函数是没有问题的，可以正常的运行，我们需要分析一下上面的函数中的`this`的指向的问题，对于箭头函数来说，`this`的指向看的上下文，现在我们可以看到这个上下文是`clock`的这个实例，上面一定有这个方法，所以不会报错。
那我们看一下下面的这个写法：
```javaScript
class Clock extends React.Component {
  state = {date: new Date()};
  onChange (){
    this.setState({data:111})
  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
        <Button onClick = {this.onChange()}>确认</Button>
      </div>
    );
  }
}
```
上面的函数是`function`的方式来写这个函数，我们知道对于这样的方法，我们是谁调用这个方法`this`就会指向谁，我们通过`console.log（）`打出发现this指向的是undefined，其实是` <Button onClick = {this.onChange()}>确认</Button>`这个`dom`，但是由于在`react`中，`dom`节点为虚拟`dom`，所以最后打出的是`undefined`，所以我们这样使用是有问题的。
### `super`中的`props`是否必要？ 作用是什么？？
- 可以不写`constructor`，一旦写了`constructor`，就必须在此函数中写`super()`,
- 此时组件才有自己的`this`，在组件的全局中都可以使用`this`关键字，
- 否则如果只是`constructor` 而不执行` super() `那么以后的`this`都是错的！！！

`super`关键字，它指代父类的实例（即父类的`this`对象）。子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错。这是因为子类没有自己的`this`对象，而是继承父类的`this`对象，然后对其进行加工。如果不调用`super`方法，子类就得不到this对象。
只有一个理由需要传递`props`作为`super()`的参数，那就是你需要在构造函数内使用`this.props`