---
title: webpack-模块热替换
categories: 工具
tag:
  - 前端
  - webpack
  - JavaScript
  - 实践
toc: true
thumbnail: "/images/view.jpg"
---
#### 模块热替换(HMR)
这个功能允许在运行时更新所有类型的模块，而不是完全刷新。但是`HMR`不适合生产环境，也就是说它更加多的应用于开发环境。
<!--more-->
#### 启用HMR
我想要启用`HMR`首先要做的就是更新`webpack-dev-server`,然后在`webpack`内置`HMR`的插件，还要删掉`print`的入口奇点，因为现在已经在`index.js`模块中引用了。

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-cabd16fa62def6c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

也可以通过命令来进行上述的修改 `webpack-dev-server --hotOnly。`
现在修改`index.js`文件，当`print.js`内部发生变更的时候，告诉`webpack`接受模块的变更

![index.js](https://upload-images.jianshu.io/upload_images/13681871-7e361f721b3db99b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### HMR加载样式

我们先对`webpack`的配置文件进行修改,让我们的项目可以进行样式的加载：

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-1bb5fbcb47afd59a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在项目中添加一个`index.less`的代码。我们看一下现在的项目目录：

![项目目录](https://upload-images.jianshu.io/upload_images/13681871-f2ed5d560d4ab4fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![index.css](https://upload-images.jianshu.io/upload_images/13681871-2b525dab1807cac8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在我们使用`npm start `进行项目的启动，我们发现现在我更新颜色，点击保存，网页上的颜色就自动的修改了。












