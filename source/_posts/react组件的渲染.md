---
title: react组件的渲染
categories: react
tag:
  - 组件
  - 渲染
toc: true
thumbnail: /images/life.png
---
React有三种定义组件的方式，但是在实现上是殊途同归，具体的方式如下：
- 函数式定义的无状态组件
- es5 原生的使用`React.createClass`定义的组件
- es6 形式的`extends React.Component` 定义的组件
<!--more-->

#### 无状态组件

无状态组件一般的用途就是纯展示组件，这种之间只负责传入的`props`来展示，不涉及state的状态操作。具体的无状态函数组件，在官方文档中支出是：
`在大部分React代码中，大多数组件被写成无状态的组件，通过简单组合可以构建成其他的组件等；这种通过多个简单然后合并成一个大应用的设计模式被提倡。`
无状态函数组件的组成形式上面只有一个render方法的组件类，通过函数形式或是ES6箭头函数的形式在创建，并且在这个之间是无`state`的。
我们先举个小栗子：
```
function HelloComponent(props, /* context */) {
  return <div>Hello {props.name}</div>
}
ReactDOM.render(<HelloComponent name="Sebastian" />,Node) 
```
我们看一下上面的代码，可以看出无状态组件的形式代码的可读性会更好，并且减少了大量的冗余的代码，精简至只需要一个render方法，大大增强了编写组件的遍历，除此之外还有以下几个显著的特点：

