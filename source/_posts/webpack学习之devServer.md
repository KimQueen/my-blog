---
title: webpack学习之devServer
categories: webPack
tag:
  - webPack
toc: true
thumbnail: "/images/name.jpg"
---
我们在开发的时候，如果只是进行简单的配置，那么我们每一次修改代码，都要手动的进行打包，然后手动的刷新浏览器，这样的话，开发的效率会很低，所以我们这个时候就要想`webpack`是否可以为我们提供一种方式让`webpack`帮助我们完成上面的操作，而不是依靠手动进行处理。
<!--more-->
#### 监听的方法
在`package.json`进行相关的配置：
```JSON
  "scripts": {
    "watch": "webpack --watch",
  },
```
进行上面的配置后后，执行命令`npm run watch`我们就会直接监听源代码，当源代码发生改变，会自动为我们重新打包。
#### webpack-dev-server
我们有的时候期望，我们不仅仅可以直接监听变化的时候进行重新打包，并创建一个服务，在浏览器上的表现是自动刷新浏览器。
```javaScript
module.exports = {
  mode: 'development',
  devtool: 'chep-module-eval-source-map',
  devServer: {
    contentBase: './dist',
    open: true, // 会自动打开一个浏览器
    proxy: 8000
  },
}
```
在`package.json`进行相关的配置：
```JSON
  "scripts": {
     "start": "webpack-dev-server",
  },
```
**我们为什么要创建一个服务：**
因为我们直接在本地通过文件协议的方式，是没有办法发送`ajax`的请求的。
#### 自己配置node的书写方式，进行其服务

在`package.json`进行相关的配置：
```JSON
  "scripts": {
    "server": "node server.js"
  },
```
我们需要先安装一个中间件：`webpack-dev-middleware`
并在`package.json` 同级目录中，写一个`server.js`的文件
```javaScript
const express = require('express');
const webpack = require('webpack'); // 引入了webpack的库
const webpackDevMiddleware = require('webpack-dev-middleware');
const config = require('./webpack.config.js'); // 引入了webpack的配置文件
const complier = webpack(config); // 使用webpack和配置文件，可以随时的进行代码的编译

// 在node中使用wenpack
const app = express();

// 只要文件发生了改变，complier就会重新进行打包，打包后的文件放在publicPath路径下
app.use(
  webpackDevMiddleware(complier, {
    publicPath: config.output.publicPath
  })
);

app.listen(3000, () => {
  console.log('server is running');
});
```
