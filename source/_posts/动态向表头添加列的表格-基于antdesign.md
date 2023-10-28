---
title: 动态向表头添加列的表格-基于antdesign
categories: 前端功能实现代码
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - 表格
toc: true
thumbnail: "/images/summary1.jpg"
---
[git地址](https://github.com/KimQueen/master.git)的compont文件夹中

<!--more-->

![表格效果](/images/12.png)

使用参数的参数列表：

下拉框实现的效果和antdesign自己声明的过滤器效果一样，但是由于我们的操作逻辑和过滤的逻辑不尽相同，所以自己封装了点击下拉的那个部分。

![参数使用详情](/images/11.png)

表格源代码：
```javascript
/**
 * 
 *  dataSource用来传入表格中想要显示的数据
 *  headerSetting用来传入初始表头的选项内容 eg：[{ title: '序号', dataIndex: 'order', key: 'num' }]
 *  choice用来传入点击图标后，出现的复选框的内容 eg：[{ title: 'Male0', key: 'male0', dataIndex: 'male0' }]
 * onInquireItem={this.onInquireItem}  对应操作中的查询按钮
 * onCloseItem={this.onInquireItem}  对应操作中的关闭按钮
 * onDeleteItem={this.onInquireItem} 对应操作中的删除按钮
 */
import React, { Component } from 'react';
import { Icon, Table, Checkbox } from 'antd';
import { findIndex } from 'lodash';
import iconFont from 'Assets/iconfont.js';
import PropTypes from 'prop-types';
import './style.less';
const { Column } = Table;
const IconFont = Icon.createFromIconfontCN({
  script: iconFont
})
class HeaderSettingTable extends Component {
  state = { tableHeader: [], tableHeaderOlder: [], additionChoice: [], showChoice: false, checkState: [] }

  // 绑定鼠标的点击事件
  componentWillUnmount() {
    window.removeEventListener('mouseup', this.handleMouseUp)
  }

  // 当页面挂载的时候，将需要的数据进行整理
  componentDidMount() {
    let olderHeader = [];
    for (let i = 0; i < this.props.choice.length; i++) {
      this.state.checkState.push(false)
    }
    olderHeader = JSON.parse(JSON.stringify(this.props.headerSetting));
    window.addEventListener('mouseup', this.handleMouseUp)
    this.setState({
      tableHeader: this.props.headerSetting,
      tableHeaderOlder: olderHeader
    });
  }

  //将不必要的数据置空
  resetCheckBox = (arr, flag) => {
    let num = 0;
    for (let i = 0; i < arr.length; i++) {
      if (arr[i] === true) {
        num++
      }
    }

    if (flag && num !== this.state.additionChoice.length) {
      for (let i = 0; i < this.props.choice.length; i++) {
        let result = findIndex(this.state.additionChoice, this.props.choice[i]);
        if (result === -1) {
          arr[i] = false;
        } else {
          arr[i] = true;
        }
      }
      this.setState({
        checkState: arr
      })
    }

  }

  //点击页面空白区域,更多弹层消失
  handleMouseUp = (newData) => {
    let arr = JSON.parse(JSON.stringify(this.state.checkState));
    if (this.state.additionChoice.length === 0) {
      for (let i = 0; i < this.state.additionChoice.length; i++) {
        arr[i] = false;
        this.setState({
          checkChoice: arr
        })
      }
    }
    document.onmouseup = (event) => {
      let flag = event.target.id !== 'choiceModal' &&
        event.target.parentElement.id !== 'iconId' &&
        (event.target.parentElement.parentElement.parentNode.id || event.target.parentElement.parentElement.id) !== 'choiceModal'
      this.resetCheckBox(newData, flag);
      if (event.target.id !== 'choiceModal' &&
        event.target.parentElement.id !== 'iconId') {
        this.setState({
          showChoice: false
        })
      }
    }
  }

  // 点击多选框的时候，对多选框中的值进行查看
  checkChoice = (index, event) => {
    let newData = JSON.parse(JSON.stringify(this.state.checkState));
    newData[index] = !newData[index];
    this.setState({
      showChoice: true,
      checkState: newData
    });
    let value = JSON.parse(JSON.stringify(newData))
    this.handleMouseUp(value)
  }

  // 点击确认按钮的时候，将选中列加入到表格的表头中去
  confirmClick = () => {
    this.setState({
      tableHeader: this.state.tableHeaderOlder,
    });
    for (let i = 0; i < this.state.checkState.length; i++) {
      let index = findIndex(this.state.additionChoice, this.props.choice[i]);
      if (this.state.checkState[i]) {
        if (index === -1) {
          this.state.additionChoice.push(this.props.choice[i]);
        }
      }
      else {
        if (index !== -1) {
          this.state.additionChoice.splice(index, 1);
        }
      }
    }
    let tempArr = JSON.parse(JSON.stringify(this.state.tableHeaderOlder));
    for (let i = this.state.additionChoice.length - 1; i > -1; i--) {
      let objectItem = this.state.additionChoice[i]
      tempArr.splice(2, 0, objectItem);
      this.setState({
        tableHeader: tempArr
      });
    }
  }

  // 点击取消按钮
  clearClick = () => {
    let clearArr = [];
    for (let i = 0; i < this.props.choice.length; i++) {
      clearArr[i] = false;
    }
    let tempArr = JSON.parse(JSON.stringify(this.state.tableHeaderOlder));
    this.setState({
      tableHeader: tempArr,
      checkState: clearArr,
      additionChoice: []
    });
  }

  // 点击图标的
  clickIcon = (event) => {
    this.setState({
      showChoice: !this.state.showChoice
    })
  }
  operateCell = (text, recorder) => {
    return (
      <div>
        <a onClick={() => this.props.onInquireItem(recorder)}>查询</a>
        <a className='a-Style' onClick={() => this.props.onCloseItem(recorder)}>关闭</a>
        <a className='a-Style' onClick={() => this.props.onDeleteItem(recorder)}>删除</a>
      </div>
    )
  }

  // 下拉框的样式的编写
  titleHandle = () => {
    return (
      <div>
        <span>操作</span>
        <IconFont id='iconId' type="icon-empty-setting" className='Icon-style' onClick={() => this.clickIcon(event)} />
        <div id='choiceModal' className={this.state.showChoice ?
          'ant-table-filter-dropdown item-style show-item' : 'ant-table-filter-dropdown item-style hidden-item'} >
          {this.props.choice.map((item, index) => {
            return (<Checkbox onChange={this.checkChoice.bind(this, index)}
              className='checkBox-style' key={item.title} checked={this.state.checkState[index]}>{item.key}</Checkbox>)
          })}
          <div className='ant-table-filter-dropdown-btns line-top'>
            <a className='ant-table-filter-dropdown-link confirm' onClick={() => this.confirmClick()} >确定</a>
            <a className='ant-table-filter-dropdown-link clear' onClick={() => this.clearClick()} >重置</a>
          </div>
        </div>
      </div >
    )
  }

  render() {
    return (
      <div>
        <Table dataSource={this.props.dataSource} scroll={{ x: this.state.tableHeader.length * 150 + 200 }} >
          <Column key='normalNum' title='序号' width={80} fixed={'left'} render={(text, recorder, index) => <span>{index + 1}</span>} />
          {
            this.state.tableHeader.map((item) => {
              return (<Column key={item.key} className={'Column-width'} title={item.title} dataIndex={item.dataIndex} />)
            })
          }
          <Column key='ok' title={this.titleHandle()} width={136} fixed={'right'} render={(text, recorder) => this.operateCell(text, recorder)} />
        </Table>
      </div >
    )
  }

}
HeaderSettingTable.propTypes = {
  headerSetting: PropTypes.array,
  onInquireItem: PropTypes.func,
  onCloseItem: PropTypes.func,
  onDeleteItem: PropTypes.func,
  choice: PropTypes.array,
  dataSource: PropTypes.array
}
export default HeaderSettingTable;
```