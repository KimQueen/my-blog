---
title: React生命周期的图解
categories: react
tag:
  - 前端
  - react
  - JavaScript
toc: true
thumbnail: "/images/exam.jpg"
---
## 生命周期的概括图

在hook出现之前，生命周期是很重要的概念，我们今天看一下生命周期一般都是在上面阶段来进行调用：

<!--more-->

![生命周期](/images/1.png)

## 生命周期的划分
### 初始化阶段
- `defaultProps` 设置组件的默认属性
- `constructor` 设置组件的初始化状态
- `componentWillMount（）`即将被废弃的声明周期
- `render()`组件渲染
- `componentDidMount() `  组件已经被渲染到页面中后触发

`componentWillMount`被废弃的理由：如果在`componentWillMount`中发送异步请求，在`SSR`（服务端渲染）的情况下，服务端与客户端共用一套组件原代码，此时会发出两次请求（服务端请求一次、客户端请求一次），服务端的请求是多余的。如果将异步请求放在`componentDidMount`中，服务器不会执行`componentDidMount`生命周期函数，可以减少不必要的请求。
`componentDidMount`:另外提醒在這邊綁定`DOM eventListener`，記得在`willUnMount`取消綁定`EventListener`，如果重新`render`元件會再次執行`DidMount`，造成過多的綁定事件。

### 运行阶段
- `componentWillReceiveProps(nextProps) `组件接收到属性时触发,丢弃
- `shouldComponentUpdate(nextProps, nextState)`
- ` componentWillUpdate(nextProps, nextState)`组件即将被更新时触发，丢弃
- `componentDidUpdate(nextProps, nextState)` 组件被更新完成后触发。页面中产生了新的`DOM`的元素，可以进行`DOM`操作

`componentWillReceiveProps(nextProps)`:会回传更新过的`props`,并且可以使用`setState`来更新`state`，但是这里使用`setState`，并不会重新执行`componentWillReceiveProps`，因为`ReceiveProps`只会在更新传递的`props`的时候进行调用。

`shouldComponentUpdate`:当组件接收到新属性，或者组件的状态发生改变时触发。组件首次渲染时并不会触发,这个生命周期的返回值是布尔值，会显示后续是否进行重新的渲染

`componentWillUpdate`：在这个生命周期使用`setState`，会导致重新跑回到`updata`的生命周期，然后在跑到`componentWillUpdate`，如果不慎用的话，会导致反复的执行。

```
shouldComponentUpdate(newProps, newState) {
    if (newProps.number < 5) return true;
    return false
}
//该钩子函数可以接收到两个参数，新的属性和状态，返回`true/false`来控制组件是否需要更新。
```
我们使用这个生命周期一般用于优化，有的时候，我们在界面上只需要更新一个很小的组件，而一个父组件的更新会造成整个子组件都进行渲染，形成一个崭新的虚拟`DOM`，但是这样的话，会造成资源的浪费，我们可以根据实际的开发情况在`shouldComponentUpdate()`生命周期加入条件，来进行性能的优化。
### 销毁阶段
- `componentWillUnmount() `有的是对轮询的请求的清理，有的是对定时器的清理，根据实际情况来订。

## 将要废弃的声明周期
- componentWillMount() -> 17版废弃
- componentWillReceiveProps(nextProps) -> 17版废弃
- componentWillUpdate（nextProps, nextState)  -> 17版废弃