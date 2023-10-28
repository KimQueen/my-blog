---
title: antdesign实现表格的轮播
categories: 前端功能实现代码
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - 表格
  - antdsign
  - react
toc: true
thumbnail: "/images/see.jpg"
---
我的应用场景是：每5分钟获取一次120条数据，每一次表格显示两条数据，每5s进行轮播一次。如果所有数据都播放完成，将会从头开始轮播，但是一旦请求的数据获取到了，就会打断之前的轮播，进而轮播新的数据。

<!--more-->
![表格的样式](https://upload-images.jianshu.io/upload_images/13681871-bfc1abf63cde635e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们在这里使用的antdesign的表格，稍微将表格的样式进行修改，并去掉了分页，具体的引用如下：

![表格使用](https://upload-images.jianshu.io/upload_images/13681871-f522c618ae75eee1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中dataSource 是表格的数据，根据自己的情况来定，loading是加载动画，可以根据自己的情况进行自定义的定制，columns是表头的设置，可以根据antdesign的说明文档进行详细的了解，因为在我的页面中不需要做分页，所以我将pagination这个属性设置为false。


被模糊掉的是数据的部分。


![对下面要用到的是进行实例化](https://upload-images.jianshu.io/upload_images/13681871-4eb621869e74c5e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![实现的源码](https://upload-images.jianshu.io/upload_images/13681871-c0669c07a1bea85f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中有两点是需要注意的：
- 两个定时器要是平级的，一般在编码的时候，不能讲定时器进行嵌套，否则会出现时间的紊乱
- init 主要的作用就是当请求回来的时候，进行新的数据的轮播

上面的代码我是在componentDidMount里面写的，通过setInterval的方法实现轮播，大家如果有跳转的需求，在组件将要挂载的时候，将上面的定时器清除掉，否则会影响性能。
