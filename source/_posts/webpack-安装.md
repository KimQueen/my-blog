---
title: webpack-安装
categories: 工具
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - webpack
  - JavaScript
  - 实践
toc: true
thumbnail: "/images/tree.jpg"
---
#### 安装webpack
我现在本地搭建一个项目，来研究`webpack`的用法，先用`git bash` 新建一个项目，这样的目的是为了生成一个`package.json`的文件，有了这个包我们才可以进行后续的装包。
```
npm init -y 
// 如果没有额外的消息要写的话
// 可以添加-y参数，想要进行详细的配置的话可以去掉-y
```

<!--more-->
执行后我们得到的项目目录为：

![对webpack进行配置](https://upload-images.jianshu.io/upload_images/13681871-bdc870fbe5d8a0c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
npm install --save-dev webpack // 直接安装
npm install --save-dev webpack@<version> // 指定版本进行安装
```
安装过之后，然后按照要求新建两个文件，现在的项目目录如下：

![新建的文件的目录](https://upload-images.jianshu.io/upload_images/13681871-f5f056ac1da210a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对`index.js`和`index.html`进行编写，编写的内容如下：

![index.html](https://upload-images.jianshu.io/upload_images/13681871-67c1ab21e5489849.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![index.js](https://upload-images.jianshu.io/upload_images/13681871-51bda7af3fee0308.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对`package.json`进行调整

![package.json](https://upload-images.jianshu.io/upload_images/13681871-aa76221a6cb7f55e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 创造一个bundle
我们将上面项目进行目录调整，调整的结果为：

![对目录结构进行调整](https://upload-images.jianshu.io/upload_images/13681871-702ef259b2d10857.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们为了避免断网的情况，将`loash`的依赖安装进来，安装`lodash`依赖的语句是：
```
npm install --save lodash
```
我们现在看一下修改过目录结构之后，我们的代码变成了如下的样式：

![index.js](https://upload-images.jianshu.io/upload_images/13681871-b5a740c36f85677b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![index.html](https://upload-images.jianshu.io/upload_images/13681871-7910abb0531d992c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我删除了上面显示的引入` <script src="https://unpkg.com/lodash@4.16.6"></script>`这一行的代码，然后将`index.js` 改成了`main.js`,然后执行`npx webpack` 打包成功后访问，在浏览器中打开` index.html`，如果一切正常，你应该能看到以下文本：`Hello webpack`。当然在打包过程中会有一定的警告 ，但是这并不影响使用。

#### 使用配置文档
先添加一个用来配置`webpack`的配置文档

![配置文档](https://upload-images.jianshu.io/upload_images/13681871-62e805a6907b7d56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

配置文档的内容如下：

![配置文档的配置信息](https://upload-images.jianshu.io/upload_images/13681871-b537ab847c7532ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们执行命令`npx webpack --config webpack.config.js`来打包这个文档，打包成功，在界面上显示`Hello webpack`的字样。
#### npm scripts
我们可以对指定的命令进行封装

![script的封装](https://upload-images.jianshu.io/upload_images/13681871-32e0268b5a2cfc13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样的话我们就可以不使用`npx webpack`  而是`npm build`



