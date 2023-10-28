---
title: antDesign+react表单验证注意事项
categories: react
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - react
  - antDesign
toc: true
thumbnail:  "/images/safe.jpg"
---

我们先用下面的代码实现一下我们要求的表单：
<!--more-->

![表单验证](/images/13.png)

对于表单验证在`antdesign`里面有非常详细的描述，我主要描述的是对于相关的布局的问题，比如说我们想要实现下面的这个在点击‘测试连接’按钮的时候，进行表单的验证。先写一下逻辑代码：
 ```javaScript
class TestConnect extends Component{
  static propTypes ={
    form:PropsType.object
  };
  //点击保存的时候进行验证
  handleSubmit = e =>{
    e.preventDefault();
    this.props.form.validateFields((err,values) =>{
      if(!err){
        console.log('receive the value of input ' + values);
      }
    })
  }

  render(){
    const {getFieldDecorator} = this.props.form;
    const formItemLayout ={
      label:(
        <span>
          <b>备份路径</b>备份集保存的远程路径，支持nfs协议
        </span>
      ),
      wrapperCol:{span:17}
    };
  return(
    <Row>
      <Col>
        <Form onSubmit ={this.handleSubmit} span ={24} layout ='vertical'>
          <FormItem {...formItemLayout }>
            <Row>
              <Col span={12}>
                {getFieldDecorator('testPath',{
                  rules:[{required:true,message:'备份路径不能为空'}]
                })(<Input placeholder='请输入远程路径'/>)}
              </Col>
              <Col offset={1} span={4}>
                <Button htmlType ='submit' type = 'primary'>
                  测试连接
                </Button>
              </Col>
            </Row>
          </FormItem >
        </Form>
      <Col>
    </Row>
   );
  }
}
const Test = Form.create()(TestConnect);
```
对于布局来说，使用的是`grid`布局，对于`antdesign`库，提供了进行校验的接口，但是对于布局要使用`FormItem`来进行书写，这样的话，就可以避免在进行校验的时候，发生布局混乱的问题，不会出现在错误信息显示的时候，将下面的div挤下去。