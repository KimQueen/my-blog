---
title: 'webpack学习之plugins,entry,output'
categories: webPack
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - webPack
toc: true
thumbnail: "/images/green.jpg"
---
我们先使用两个插件来看一下插件到底是什么？
#### html-webpack-plugin
使用：这个插件运行时在打包结束的时候进行的
- 先在项目中安装插件，`npm install html-webpack-plugin -D `
- 在`webpack.config.js`中进行相关插件的配置

<!--more-->

```js
const path = require('path'); // node 中的内置模块路径
const HtmlWebpackPlugins = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.js', // 打包的入口文件
  plugins: [
    new HtmlWebpackPlugins({
      template: 'src/index.html;' // 当打包的时候生成文件的时候，按照template配置的html进行生成
    }),
  ],
  output: {
    filename: 'bindle.js', // 打包后输出的文件的名字
    path: path.resolve(__dirname, 'dist') // 绝对路径,__dirname是webpack.config.js所在的路径的位置,输出的文件放在bundle的文件夹下
  }
};
```
`html-webpack-plugin`作用:会在打包结束后，自动生成一个`html  `文件，并将打包生成的`js`自动引入到`html`中，当我们配置了`template`参数，会有一点区别，在打包生成`html`的时候，会根据指定的模板进行生成，并将打包生成的`bindle.js`注入到`html`文件中
####clean-webpack-plugin

```js
const path = require('path'); // node 中的内置模块路径
const HtmlWebpackPlugins = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.js', // 打包的入口文件
  plugins: [
    new HtmlWebpackPlugins({
      template: 'src/index.html;' // 当打包的时候生成文件的时候，按照template配置的html进行生成
    }),
  ],
  output: {
    filename: 'bindle.js', // 打包后输出的文件的名字
    path: path.resolve(__dirname, 'dist') // 绝对路径,__dirname是webpack.config.js所在的路径的位置,输出的文件放在bundle的文件夹下
  }
};
```
这个插件的作用就是在再次打包的时候`dist`文件夹中的内容清空

**插件：在webpack运行到某个时刻的时候，帮我们做一些事情**
#### Entry 和output

##### 基础
```js
const path = require('path'); // node 中的内置模块路径
const HtmlWebpackPlugins = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.js', // 打包的入口文件
  plugins: [
    new HtmlWebpackPlugins({
      template: 'src/index.html;' // 当打包的时候生成文件的时候，按照template配置的html进行生成
    }),
  ],
  output: {
    filename: 'bindle.js', // 打包后输出的文件的名字
    path: path.resolve(__dirname, 'dist') // 绝对路径,__dirname是webpack.config.js所在的路径的位置,输出的文件放在bundle的文件夹下
  }
};
```
这是最近简单的打包，打包成一个简单的文件，并引入到`html`中
##### 当我们想有两个入口文件的时候
```js
const path = require('path'); // node 中的内置模块路径
const HtmlWebpackPlugins = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: {
    main: 'src/index.js',
    sub: 'src/index.js'
  },
 
  plugins: [
    new HtmlWebpackPlugins({
      template: 'src/index.html;'
    }),
    new CleanWebpackPlugin(['dist '])
  ],
  output: {
    filename: '[name].js', // 打包后输出的文件的名字，占位符
    path: path.resolve(__dirname, 'dist') // 绝对路径,__dirname是webpack.config.js所在的路径的位置,输出的文件放在bundle的文件夹下
  }
};

```
执行打包操作的时候，会将`main.js`和`sub.js`注入到`html`文件中
#### 我们期望在注入html的时候，在js文件前添加一个地址
```js
  output: {
    publicPath:'http://www.baidu.com',
    filename: '[name].js', // 打包后输出的文件的名字，占位符
    path: path.resolve(__dirname, 'dist') // 绝对路径,__dirname是webpack.config.js所在的路径的位置,输出的文件放在bundle的文件夹下
  }
```


