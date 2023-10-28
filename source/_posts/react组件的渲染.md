---
title: react组件的渲染
categories: react
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
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
- 组件不会被实例化，整体渲染的性能的得到了提升
  我们对组件的实现仅仅依靠一个render函数来进行实现，因为是没有状态的，所以也就不存在组件实例化的过程，无实例化过程也就说明了，组件是不需要进行多余的内存分配的，所以性能上会有一定的优化
- 组件是不能访问`this`的
  我们在进行组件封装的时候，在原理上面一般是先对这个类进行实例化，`let a = new A()`类似这样的过程，然后`this`指向的`a`,但是纯函数的话也就不存在这个过程了。
- 组件没有办法访问声明周期的方法
  因为无状态组件是不需要组件声明周期管理和状态管理，所以底层实现这种形式的组件时是不会实现组件的生命周期方法。所以无状态组件是不能参与组件的各个生命周期管理的。
- 无状态组件只能访问输入的props，同样的props会得到同样的渲染结果，不会有副作用
  这也就意味着我们使用纯函数的方法来写组件的话，这个组件的作用一般来说就是进行界面的显示。

无状态组件被鼓励在大型项目中尽可能以简单的写法来分割原本庞大的组件，未来React也会这种面向无状态组件在譬如无意义的检查和内存分配领域进行一系列优化，所以只要有可能，尽量使用无状态组件。

#### React.Component
React.Component是以ES6的形式来创建react的组件的。
```
class InputControlES6 extends React.Component {
    constructor(props) {
        super(props);

        // 设置 initial state
        this.state = {
            text: props.initialValue || 'placeholder'
        };

        // ES6 类中函数必须手动绑定
        this.handleChange = this.handleChange.bind(this);
    }

    handleChange(event) {
        this.setState({
            text: event.target.value
        });
    }

    render() {
        return (
            <div>
                Type something:
                <input onChange={this.handleChange}
               value={this.state.text} />
            </div>
        );
    }
}
InputControlES6.propTypes = {
    initialValue: React.PropTypes.string
};
InputControlES6.defaultProps = {
    initialValue: ''
};
```

#### React.createClass
```
var InputControlES5 = React.createClass({
    propTypes: {//定义传入props中的属性各种类型
        initialValue: React.PropTypes.string
    },
    defaultProps: { //组件默认的props对象
        initialValue: ''
    },
    // 设置 initial state
    getInitialState: function() {//组件相关的状态对象
        return {
            text: this.props.initialValue || 'placeholder'
        };
    },
    handleChange: function(event) {
        this.setState({ //this represents react component instance
            text: event.target.value
        });
    },
    render: function() {
        return (
            <div>
                Type something:
                <input onChange={this.handleChange} value={this.state.text} />
            </div>
        );
    }
});
InputControlES6.propTypes = {
    initialValue: React.PropTypes.string
};
InputControlES6.defaultProps = {
    initialValue: ''
};

```
可以访问组件的生命周期方法但是随着React的发展