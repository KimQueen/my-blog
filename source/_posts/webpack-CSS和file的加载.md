---
title: webpack-CSS和file的加载
categories: webpack
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - webpack
  - JavaScript
  - 实践
toc: true
thumbnail: "/images/green.jpg"
---
我们之前一节都是对一些js的资源进行打包，但是如果我们的项目中还有一些其他的资源呢？比如说：`images`。
在`webpack`中除了可以引入`JavaScript`，还可以通过`loader`引入其他类型的文件。主要的实现是使用`loader`来实现的。
加载各类资源用到的`module`
- 加载`css`文件需要 `style-loader css-loader`
- 字体和图片需要 `file-loader`
-  `CSV`、`TSV `和` XML` 需要`csv-loader`和`xml-loader`

<!--more-->

#### 安装

![文件目录](https://upload-images.jianshu.io/upload_images/13681871-8dccf40560a8c44d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在对代码进行修改后

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-1337258be05810a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![index.js](https://upload-images.jianshu.io/upload_images/13681871-b55861d90acca599.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 加载CSS
为了在`JavaScript`模块中的import一个`CSS`文件，需要安装 [style-loader](https://webpack.docschina.org/loaders/style-loader) 和 [css-loader](https://webpack.docschina.org/loaders/css-loader)这两个插件，在`module`的配置中添加这些`loader`
```
npm install --save-dev style-loader css-loader
```
并在`webpack.config.js`的文件中的`module`进行相关的修改：

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-e6b77c5a9bb1e63f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加一个`css`文件，用来使用样式

![文件目录](https://upload-images.jianshu.io/upload_images/13681871-07a991e97acd9030.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![index.css](https://upload-images.jianshu.io/upload_images/13681871-fa2df95a3770a75d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对`index`的文档也进行相对应的修改

![index.js](https://upload-images.jianshu.io/upload_images/13681871-6e0d83eff5d24c89.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 加载images图像

在加载`css`的时候，我们也可能需要`background`会让`icon`这样的图片，但是我们要要如何处理呢？在这个部分我们使用的是`file-loader`，我们就可以将这些内容混合到`css`里面了。
```
npm install --save-dev file-loader
```
对`webpack`的配置如下：

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-4b6687650eacc546.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们尝试向项目中添加一个图像，看看它具体是如何工作的，现在项目中添加一个图片：

![项目目录](https://upload-images.jianshu.io/upload_images/13681871-7213914a0cc590f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![index.js](https://upload-images.jianshu.io/upload_images/13681871-188f5991bdc6c482.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![index.less](https://upload-images.jianshu.io/upload_images/13681871-6b8183ce1958c755.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后执行打包的命令，打包成功后可以在网页中看到该界面。
#### 加载数据
有的时候我们请求的资源或是数据是内置，如`JSON`文件，`CSV`，`TSV`和`XML`。我们有一个例子比如说我们要本地的`json`文件，因为加载`CSV`、`TSV`和`XML`需要用到`CSV-loader`和`xml-loader`，让我们来加载这三类文件。
```
npm install --save-dev csv-loader xml-loader
```
![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-e0f504dad463f392.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在在项目目录添加一个`data.xml`文档：

![新的项目目录](https://upload-images.jianshu.io/upload_images/13681871-de42771f38d46a28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![data.xml](https://upload-images.jianshu.io/upload_images/13681871-2c3f4321b36aadbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以 `import` 这四种类型的数据(`JSON, CSV, TSV, XML`)中的任何一种，所导入的 `Data` 变量，将包含可直接使用的已解析 `JSON`：

![index.js](https://upload-images.jianshu.io/upload_images/13681871-08bdac16c90333fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行`npm run build`然后打开`index.html`,在控制台看一下打出控制台，我们可以看到导入的结果。

![Data的输出结果](https://upload-images.jianshu.io/upload_images/13681871-e3ca5e1bd1b192a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



