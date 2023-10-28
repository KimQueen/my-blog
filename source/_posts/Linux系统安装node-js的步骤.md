---
title:  Linux系统安装node.js的步骤
categories: 工具
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - 工具
toc: true
thumbnail: "/images/book.jpg"
---
#### 下载自己需要的文件系统配置：
 英文网址：https://nodejs.org/en/download/
 中文网址：http://nodejs.cn/download/

我们可以通过`uname -a`的方式 来查看我们的Linux系统的位数是64位
（ps：`x86_64表示是64位的系统 i686 1386代表是32位的系统`）
<!--more-->

![查询系统的配置](https://upload-images.jianshu.io/upload_images/13681871-9dca87c93404e1f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![中文网站的显示](https://upload-images.jianshu.io/upload_images/13681871-2879c4a04c15e465.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 下载下来的tar文件上传到服务器并且解压，然后通关简历软连接编程全局
1） 上传服务器可以是自己的任意路径，目前我存放的路径是`root/`
2) 解压上传的文件（解压后的文件我将文件的名字改为`node.js`，这个地方可以自己随意，只要建立软连接的时候书写正确就可以）
` tar -xvf   node-v10.15.3-linux-x64.tar.xz `
`mv node-v6.10.0-linux-x64  nodejs `
确认一下`nodejs`下`bin`目录是否有`node` 和`npm`文件，如果有执行软连接，如果没有重新下载执行上边步骤
3）建立软连接，变为全局
   ① `ln -s /root/nodejs/bin/npm /usr/local/bin/ `
   ② `ln -s /root/nodejs/bin/node /usr/local/bin/`
4)使用`node-v`显示当前的`node`的版本
5）安装yarn
`npm install -g yarn`