---
title: antDesign+react获取输入框中的值
categories: react
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - react
  - antDesign
toc: true
thumbnail: "/images/train.jpg"
---
具体的应用的场景是，在输入的时候对数据进行获取数据，在保存的时候，将获取的数据进行打印：

<!--more-->

![获取输入框中的数据](/images/18.png)

具体的代码实现：
```javaScript
import React,{Fragment,PureComponent} from 'react';
import moment from 'moment';  //引入了moment的库
import {Input} from 'antd';
class InputSave extends PureComponent{
  state ={value1:'',value2:''}

//在输入框发生变化的时候修改状态的值
  handleMaxRestoreUp = ()=>{
    if(event && event.target && event.target.value){
      let value = event.target.value;
      this.setState(()=>({value1:value }))
    }
  }

//在输入框发生变化的时候修改状态的值
  handleMaxBackUp= ()=>{
    if(event && event.target && event.target.value){
      let value = event.target.value;
      this.setState(()=>({value1:value }))
    }
  }

//点击保存的时候打出输入框中的值
  saveData=()=>{
    console.log(this.state.value1);
    console.log(this.state.value2);
  }
  render(){
    return(
      <div>
        <span>最大的备份速度</span>
        <Input defaultValue ='20' onChange ={event => this.handleMaxBackUp(event)}/>
        <span>MB 最大恢复速度</span>
        <Input onChange ={event => this.handleMaxRestoreUp(event)} />
        <span>MB</span>
        <div>
          <Button type ='primary' onClick{()=>{this.saveData()}}>保存</Button >
        </div>
      </div>)
  }
}
```
