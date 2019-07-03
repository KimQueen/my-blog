---
title: webpack-开发环境
categories: 工具
tag:
  - 前端
  - webpack
  - JavaScript
  - 实践
toc: true
thumbnail: "/images/window.jpg"
---
#### 开发环境
<!--more-->

我们在`webpack`的配置文件中将`mode`设置成`development`,具体的代码如下：

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-1d6c4e64d54133da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### source map
当我们使用`webpack`打包源代码的时候，可能会很难最终到`error`和`warning`的位置，例如，我们现在有三个源文件打包到了一个`bundle`中，而其中有一个源文件错误了，那么报错的时候，会直接指向`bundle.js` 。我们想要准确的知道错误来自某一个源文件，这样的提示信息往往没有什么实际意义。
为了我们可以更加轻松的追踪错误和警告，`JavaScript`提供了`source map`功能，可以将编译后的代码映射到原始源代码。我们看一下下一个例子。首先对`webpack`的配置文件进行配置：

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-c3f3140f28018c34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们在`print.js`里面的代码在语法上有错误

![print.js](https://upload-images.jianshu.io/upload_images/13681871-43461b0b01606f2f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们进行打包，我们成功打包之后我们点击按钮，点击后在控制台会输出如下的错误：

![控制台输出错误](https://upload-images.jianshu.io/upload_images/13681871-b83d3cbe0846b3df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

方便我们进行简单的错误定位。


- `webpack watch mode`(webpack 观察模式)
- ` webpack-dev-server `（最可能被用到）
- `webpack-dev-middleware`
####  watch mode(观察模式)
如果其中一个文件被更新，代码将被重新编译，所以你不必再去手动运行整个构建。
但是这里需要我们手动的启动`webpack  watch mode的npm scripts`我们看一下这里是如何进行配置的：

![package.json](https://upload-images.jianshu.io/upload_images/13681871-d89e4c2e21c554b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在我们执行命令`npm run watch` 我们会进入到`watch`模式下，但是和`build`指令不同，这个执行没有退出，也就是我们还在一直处于被监控的状态，我们在修改文件，点击保存的时候，就会直接进行编译。

#### 使用 webpack-dev-server

我们看一下我们应该怎么使用，首先我们需要先安装一下。
```
npm install --save-dev webpack-dev-server
```
在`webpack`的配置文件中进行配置：

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-38b24339d846ee7e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面怎么配置的意思是将dist目录下的文件serve到localhost:8000下，这个时候我们可以添加一个可以直接运行`dev serve`的`script`，在`package.json`中

现在，在命令行中运行 `npm start`，我们会看到浏览器自动加载页面。如果你更改任何源文件并保存它们，`web server` 将在编译代码后自动重新加载

#### 使用 webpack-dev-middleware

`webpack-dev-middleware` 是一个封装器(`wrapper`)，它可以把` webpack` 处理过的文件发送到一个 `server`。 `webpack-dev-server` 在内部使用了它，然而它也可以作为一个单独的 `package` 来使用，以便根据需求进行更多自定义设置。下面是一个 `webpack-dev-middleware` 配合 `express server` 的示例。

首先我们需要先安装`express`和`webpack-dev-middleware`
因为我的网速比较慢，这里我是使用`yarn` 来进行安装的，安装的命令为：
```
yarn add express webpack-dev-middleware
```
接下来我们调整`webpack`的配置文件，确保`middleware` 功能是可以正常使用的

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-cb8dbd8a0af93bb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们在脚本中使用了`publicPath`,用来确认资源文件可以正确的在`http://localhost:3000`，我们也可以指定`port number`，下面我们自定义`express server`：
先创建一个项目目录：

![项目目录](https://upload-images.jianshu.io/upload_images/13681871-66dd700f00fc8a25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![server.js](https://upload-images.jianshu.io/upload_images/13681871-4206dabe8adae041.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们添加一个`npm script `，让我们可以方便的运行服务：

![package.json](https://upload-images.jianshu.io/upload_images/13681871-2725ae83c2bfa01c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们执行`npm run server` 的时候，成功之后，我们访问`http://localhost:3000`就看到了我们写的界面。



