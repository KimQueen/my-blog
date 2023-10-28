---
title: antDesign方法默认传值加上其他参数传值
categories: 前端功能实现代码
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - 表格
  - antdsign
  - react
toc: true
thumbnail: "/images/fly.jpg"
---
`Ant design`里面有一些触发方法，如：`OnChange`,`OnClick`等等，等到我们触发的时候，这个触发函数就会自动传瑞一些值给方法。
比如`Switch`的`OnChange`方法调用，即使你什么参数都不传入，也会有一个默认的`Boolean`值传入，这个布尔值标识现在开关的状态，但是有的时候我们还需要传入一些固定值，也就是我们自己想要传入的值怎么办呢？

<!--more-->
如果以直接写：
```JavaScript
onChange={this.onChange(你要传的参数)}
```
他会用你要传的参数覆盖掉默认值`value`，这样你就不能把`value`传过去。
如果写成：
```JavaScript
onChange={this.onChange(value,你要传的参数)}
```
他会提示你`value`值没有定义。如果想要将这两个值都传进去的话现阶段有两个方法：
```
//用bind，this后面加上你要的参数，他会把value值传到你写的方法的最后一个参数上
onChange={this.onchange.bind(this,你要传的参数)}  

// 显式地把value写出来，这样就可以把value和参数都传过去
onChange={(value)=>{this.onchange(value,你要传的参数)}} 
```