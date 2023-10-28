---
title: antDesign+react实现选择不同的下拉框出现不同的组件
categories: react
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - react
  - antDesign
toc: true
thumbnail: "/images/name.jpg"
---
在选择不同的下拉框的时候出现不同的组件，具体的实现效果就是：
<!--more-->

![初始状态](/images/14.png)

![选择每天运行时的效果](/images/15.png)

![选择每周运行时的效果](/images/16.png)

![选择每月运行时的效果](/images/17.png)

我具体的做法是，在第一个下拉框作为一个控制组件，后面的显示结果通过判断第一个下拉框选择的不同值进行显示的划分。
首先先对第一个组件进行封装,，该文件的文件名的index.js：
```JSX
import React,{PureComponent} from 'react';
import {Select} from 'antd';
import SwitchInterval  from '/SwitchInterval '
const Option = Select.Option ;

class FirstSelect extends PureComponent {
  state = {value:''};

  //   向子组件传值
  handleChange = value =>{
    this.setState(()=>({value}));
  }
  render(){
    const {value} =this.state;
    return(
      <div>
        <Select value ={value} onChange ={this.handleChange}>
          <Option value =''>请选择</Option >
          <Option value ='day'>每天运行</Option >
          <Option value ='week'>每周运行</Option >
          <Option value ='month'>每月运行</Option >
        </Select}>
        <SwitchInterval />
      </div>)
  }
}
```
现在我们对子组件进行封装，子组件的文件名为SwitchInterval.js ：
```javaScript
import React,{PureComponent} from 'react';
import {Select} from 'antd';
import SwitchInterval  from '/SwitchInterval '
const Option = Select.Option ;

// 对日期组件进行封装
class DayShow extends PureComponent{
  InitTime =() =>{
    let timeValues =[];
    for(let i = 0; i< 24; i++){
      timeValues.push(i+':00');
    }
    return timeValues;
  };

  render(){
    return(
      <div>
        <span>时间为</span>
        <Select defaultValue ='0:00'>
          {this.InitTime().map(value =>(<Option key ={value} value ={value}>{value}</Option>))}
        </Select>
      </div>
    );
  }
}

// 对星期组件进行封装
class WeekShow extends PureComponent{
  InitWeek =() =>{
    let weekValues =[
      {value:1,label:'星期一'},
      {value:2,label:'星期二'},
      {value:3,label:'星期三'},
      {value:4,label:'星期四'},
      {value:5,label:'星期五'},
      {value:6,label:'星期六'},
      {value:7,label:'星期日'},
    ];
    return weekValues ;
  };

  render(){
    return(
      <div>
        <span>日期为</span>
        <Select defaultValue ='星期一'>
          {this.InitWeek().map(({value,label}) =>(<Option key ={value} value ={value}>{label}</Option>))}
        </Select>
      </div>
    );
  }
}

// 对月份组件进行封装
class MonthShow extends PureComponent{
  InitMonth =() =>{
    let monthValues =[];
    for(let i = 1; i< 32; i++){
      timeValues.push(i);
    }
    return monthValues ;
  };

  render(){
    return(
      <div>
        <span>日期为</span>
        <Select defaultValue ='1'>
          {this.InitMonth().map(value =>(<Option key ={value} value ={value}>{value}</Option>))}
        </Select>
      </div>
    );
  }
}

class SwitchInterval  extends PureComponent{
  render(){
    if(this.props.value ==='day' ){
      return(
        <div>
          <DayShow />
        </div>
      );
    }else if(this.props.value ==='week' ){
      return(
        <div>
          <WeekShow />
          <DayShow />
        </div>
      );
    }else if(this.props.value ==='month'){
      return(
        <div>
          <DayShow />
          <MonthShow />
        </div>
      );
    }else{
      return null;
    }
  }
}
export default SwitchInterval ;
```









