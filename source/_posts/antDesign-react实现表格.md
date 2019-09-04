---
title: antDesign+react实现表格
categories: react
tag:
  - react
  - antDesign
toc: true
thumbnail: "/images/tree.jpg"
---
我们想要开发一个这样的表格，那么我们要怎么通过ant design和react来进行开发
<!--more-->
![表格样式](/images/19.png)

```javaScript
import React,{Fragment,PureComponent} from 'react';
import moment from 'moment';  //引入了moment的库
import {Table} from 'antd';
const {column} = Table;

class App extends PureComponent{
//测试数据，但是key值一定要有，否则会有warning出现
let testValue =[{
  key:'1'
  startTime:'1546916859979',
  endTime:'1546916859990',
  backup:'20190111136858',
  backupNoSize:'52461',
  status:'SUCCESS'
},
{
  key:'21'
  startTime:'1546916856979',
  endTime:'1546916856990',
  backup:'201901111375212',
  backupNoSize:'52481',
  status:'FAILD'
}]
  state ={data:testValue } //我们想要在表格中显示的数据

// 对于时间进行标准化的表示
  formateDate = text =>{
    if(text !== '' && text !==undefined && text !== null && text !== 'none'){
      text = moment(pareseInt(text)).formate('YYYY/MM/DD hh:mm:ss');
      return text;
    }else{
      text ='--';
      return text;
    }
  }

// 对操作进行处理
operateCell =(text,recorder) =>{
  switch (recorder.status){
    case 'SUCCESS':
      return (<div><a>恢复</a><a>删除</a></div>)
    default：
      case:(<a>删除</a>)
  }
}
  render(){
    return (
      <Table dataSource ={this.state.data}>
        <Column title ='序号' dataIndex='backupNo' render ={(text,recorder,index) => <span>{index +1}</span>}/>
        <Column title ='开始时间' dataIndex='startTime' render ={text =>this.formateDate(text)}/>
        <Column title ='开始时间' dataIndex='endTime'  render ={text =>this.formateDate(text)}/>
        <Column title ='备份集' dataIndex='backup'/ >
        <Column title ='备份大小' dataIndex='backupNoSize' />
        <Column title ='状态' dataIndex='status' />
        <Column title ='操作' dataIndex='operate' render ={(text,recorder) =>this.operateCell(text，recorder)}/>
    )
  }
}
```
