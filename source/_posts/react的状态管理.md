---
title: react的状态管理
categories: 前端功能实现代码
tag:
  - react
  - state
toc: true
thumbnail: "/images/map.jpg"
---

在使用`react`的状态管理的过程，如果我们不使用封装好的状态管理工具的话，我们就需要对`state`进行状态的提升的时候就会涉及到两种情况：
- 父组件向子组件传值
- 子组件向父组件传值
<!--more-->

首先，我们先看一下父组件如何向子组件传递值，这个方式很简单，就是直接从父组件以`props`的方式传递到子组件中。

![父子组件的结构](https://upload-images.jianshu.io/upload_images/13681871-846b786ee4aee803.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个时候，我们引入父组件的方式，假设父组件是一个`form`的表单，子组件是一个自己封装的名为`BackupTime`的子组件
在父组件中：
```JavaScript
import BackupTime  from './BackupTime.js'

class Parent extend Component  {
data ={a:1}

  render(){
     return (
      <Form>
          <FormItem>
            <BackupTime choice ={this.data}  />
          </FormItem>
      </From>
    )
  }
}

```
在子组件中：
```JavaScript
class BackupTime  extends Component {
  compnentDidMount(){
    const {choice} = this.props
  }
}
```
这样就简单的实现了父组件向子组件传值的过程，那么如何实现子组件向父组件传值呢，比如说 我的子组件有一个下拉框，在子组件修改了这个下拉框的值，但是我们要在父组件中得到相对应的下拉框的值。

![子组件向父组件传值](https://upload-images.jianshu.io/upload_images/13681871-eb060dd5627cf222.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面的场景可以概括为，子组件的下拉框发生改变的时候，父组件的在点击`click`按钮的时候，可以将子组件的`select`选中的数据打出
```JavaScript
import BackupTime  from './BackupTime.js'

class Parent extend Component  {
  state ={select:''}
  handleSubmit =() =>{
    console.log(this.state.select)
 }
  getValue  = (value) =>{
    this.setState({select:value})
  }
  render(){
     return (
      <Form>
          <FormItem>
            <BackupTime choice ={this.data}   getValue ={this.getValue }/>
            <Button onClick={this.handleSubmit}  />
          </FormItem>
      </From>
    )
  }
}

```
在子组件中：
```JavaScript
class BackupTime  extends Component {
  compnentDidMount(){
    const {choice} = this.props
  }
  render(){
    return (
    <select  onChange ={value =>this.props.getValue(value)}/>
  )
  }
}
```
然后我们再来扩展一下我们有三种写函数的方法
```JavaScript
const A (){....} // 方法一
A = () =>{....} // 方法二
```
上面的两种函数的写法，主要体现在this的引用上面，方法一是谁调用函数，`this`就指向谁，方法二是需要看上下文
我们在`react`的组件中调用方法的写法有三种，我们来看一下
```JavaScript
<Button onClick = {this.A}> // 方法一
<Button onClick = {(value)=>this.A(value)}> //方法二
<Button onClick = {this.A()}> //方法三
```
对于方法一来说 `onClick` 等价于`this.A`这个方法，所以在这个过程中，并不会执行`A`的这个方法
方法二和方法一很相似，但是 这个里面可能需要传入除了组件默认传的值之外的值，我们使用了一个匿名函数包裹了`A`函数，所以这个`A`还是也是不会被立即执行的。
方法三，我们在`render`进行渲染的时候，就直接的调用了上面的这个方法，这个情况用于闭包较多。

ps:对于所有的复杂类型都是有`__proto__`属性，但是只有是原生方法才会有`protoType`这个属性，箭头方法也是没有`protoType`这个属性的，构造方法一般挂在`protoType`这个属性上面。所以，对于需要new的方法是没有办法使用箭头函数的。

导出文件的时候：
export default {A} :这种方式导出的组件是没有名字的，所以在引用的时候，需要对其进行重命名
export {A}：这种方式进行导出的话，由于是有名字的，所以在导出的时候要加{},相当于对变量的结构


对类型判断的时候，尽量使用`Array.isArray()`方法，避免使用`instance of`方法，因为instance of取到的时候原型上面的东西，如果被别人不小心修改了，容易出现错误
在`react`中
```JavaScript
class BackupTime  extends Component {
 A(){}  // 原型上的方法
 B =() =>{} // 实例方法,重点在于那个等于号
 B = function(){} // 实例方法
 static C (){} // 静态方法  
  render(){
    return (
    <select  onChange ={value =>this.props.getValue(value)}/>
  )
  }
}
```