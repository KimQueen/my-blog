---
title: mock的配置
categories: 工具
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - git
  - 工具
toc: true
thumbnail: "/images/brain.jpg"
---

Mock.js 是一款前端开发中拦截Ajax请求再生成随机数据响应的工具.可以用来模拟服务器响应. 优点是非常简单方便, 无侵入性, 基本覆盖常用的接口数据类型.

<!--more-->
#### 安装mock

![mock安装指令](https://upload-images.jianshu.io/upload_images/13681871-19803661117cab9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 对webpack进行配置
webpack.dev.config.js

![配置文件的配置详情](https://upload-images.jianshu.io/upload_images/13681871-8338a3932ca6f2a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### mock/indx.js的使用

利用webpack-dev-server 的before 方法调用webpack-api-mocker。path.resolve('./mocker/index.js') 中的'./mocker/index.js'为mock文件的相对
    

![mock的具体使用](https://upload-images.jianshu.io/upload_images/13681871-6f5f2b6912f1c6b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对于已经在webpack中配置public路径的童鞋注意：要在拦截url的前面加上publicurl的路径值

http://www.liubeijing.ren/article/detail/article_id/99.html
