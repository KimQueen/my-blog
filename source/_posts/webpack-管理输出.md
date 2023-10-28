---
title: webpack-管理输出
categories: 工具
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - webpack
  - JavaScript
  - 实践
toc: true
thumbnail: "/images/tree-1.jpg"
---
#### 资源输出
我们先来看一下我们的项目目录，我们新建了一个`print.js`的项目文档：
<!--more-->


![项目目录](https://upload-images.jianshu.io/upload_images/13681871-ddeb5654975a903c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们现在在`print.js`的文档中添加一些业务逻辑：

![业务逻辑代码](https://upload-images.jianshu.io/upload_images/13681871-074e030174386426.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们在`index.js`文件中使用这个函数：

![index.js](https://upload-images.jianshu.io/upload_images/13681871-546793a883fd07a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后更新`index.html`的文档，用来为`webpack`分离入口做准备：

![index.html](https://upload-images.jianshu.io/upload_images/13681871-6a1fc202f77bac8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对设置进行调整，我们将在`entry`添加`src/print.js`作为新的的入口起点（`print`），然后修改`output`，一遍根据入口奇点定义的名称，动态的更改`bundle`的名称

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-49ce8ed92cdd0d33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个时候我们可以看到`webpack`生成`print.bundle.js`和`app.bundle.js`文件，这个也是我们在`index.html`文件中指定的文件名称相对应，如果我们在浏览器中打开`index.html`,就会出现一个按钮，我们点击按钮的时候，会在控制台输出一些信息。
但是当我们有一个新的诉求，我们更改我们的一个入口奇点的名称，甚至是添加一个新的入口，会发生什么呢？会在构建的时候重新命名生成的`bundle`,但是我们在`index.html`文件中依旧引用的是旧的名称，让我们在`HtmlWebpackPlugin`来解决这个问题，
####  HtmlWebpackPlugin

首先我们先要安装一个插件。并且将`webpack.config.js`的文档进行调整：
```
npm install --save-dev html-webpack-plugin
```
我们对webpack进行相对应的配置

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-e631a34600074935.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们在构建之前，在`dist`的文件夹中已经有了`index.html`的这个文件，然而`HtmlWebpackPlugin `还会默认生成自己的`index.html`文件，也就是说，它会用新的`index.html`文件替换我们原来的文件，现在我们执行以下`npm run build`后看一下究竟会发生什么：

这个是默认生成的`index.html`的文档，它将我们之前的`index.html`文档覆盖掉了。

![index.html](https://upload-images.jianshu.io/upload_images/13681871-5df924c946e7d7fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 清理 /dist 文件夹 
我们由于多次的编译，现在的文件夹`dist`文件夹中的内容十分的杂乱。`webpack`将生成的文件放在了/文件夹中，但是他不会最终哪些资源在项目中应用，所以我们比较推荐的做法是，每一次生成的之前，先清理一下`dist`的文件夹，这样就会生成有用的文件，我们现在在下面实现一下这个场景：这里我使用了最近比较流行的插件，来配置和安装他。
```
npm install --save-dev clean-webpack-plugin
```
对`webpack`进行相应的

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-cdf26b6bc2d88e96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这是执行之前`build`命令之前的项目的文件目录：

![项目目录](https://upload-images.jianshu.io/upload_images/13681871-537eacce651c6f09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


