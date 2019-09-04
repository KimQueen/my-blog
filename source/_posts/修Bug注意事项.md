---
title: 修Bug注意事项
categories: react
tags:
  - 前端
  - JavaScript
thumbnail: "/images/tree-2.jpg"
---
1.将变量名和函数名取成了相同的名字：
有的时候，我们会不注意的时候，将变量的名字和函数的名字取成相同的名字，这样的话，会造成值的覆盖问题，导致`bug`出现。

<!--more-->
2.对于`if-else`的使用
我们条件分支里面写代码的时候，一定要注意，自己所写的代码是针对于上面条件的，尤其是`if-else if-else if-else` 最后的一个`else`中可能包含多个条件，在这个`else`中写代码的时候，是否这里的状况都符合要求。
3.对异常的处理
当我们直接拿到一个字符串就进行操作，而不考虑这个字符串的具体情况，就会出现问题，比如说：`str` 是之前得到的一个字符串，直接对其进行操作，`str.split（'-'）`，这个时候如果`str`为空或是`undefined`就会进行报错。在下面进行代码说明：
```javascript
let test (language)=>{
  let str = '';
  str = language ;
  str.split('-'); //这个时候可能会报错，解决的方法如下
  if(str !== undefined &&str !== null && str !== ''){
     str.split('-');
  }
}
```
4.对于给动态添加的`dom`节点进行修改操作
我们对于动态添加的节点再一次进行操作，要确定之前动态操作是已经生效的，否则后续的操作是可能无法生效。