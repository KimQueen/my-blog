---
title: react给axios请求配置统一的header
categories: react
tag:
  - react
  - axios
toc: true
thumbnail: "/images/color.jpg"
---

我们在开发的过程中，需要统一在请求上添加请求头。比如说我们要在请求中添加用户的`Id`。
<!--more-->
第一步：先建一个文件夹，在这个文件夹中建一个`http.js`文件
```javaScript
import axios from 'axios'; 
const init=function(){
  axios.defaults.headers.common['user'] = '111';
}
export default {init}
```
第二步：在`App`，也就是项目启动的文件中的`render`函数中调用这个方法
```javaScript
import httpTest from './http.js'
render(){
  httpTest.init();
}
```
这样的话，在下面直接调用`axios`进行发送请求的时候就会直接在发送的请求上面拼接我们需要的内容了。