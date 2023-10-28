---
title: webpack学习之基础概念
categories: webpack
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - webPack
toc: true
thumbnail: "/images/too-1.jpg"
---
#### webpack出现的背景
1.当项目中引入多个`js`文件的时候，页面加载的速度就会慢很多，因为多了`http`的请求
2.面向对象的编程方式的出现
3.如果按照传统的方式进行文件的引入，就会失去文件和文件之间的位置关系
综上所述：`webpack`是一个模块打包工具
利用`webpack`进行打包，引入模块的方式，我们一般用的有两种
- `ES module` 模块的引入方式，分别使用关键字`import`和`export` （`import`使用的前提是先`export`）
- `commonJS`的模块引入方式 
`const header = require ('./header.js')；`导入
`module.exports = Header;` 导出

<!--more-->

#### webpack的环境 ---> 给予node的打包工具
- 1. 安装`npm`,在官网上直接进行安装
- 2. 在左面创建一个`webpack`的文件夹，并进入文件夹
- 3.执行`npm init` 以`node`规范的形式创建一个项目
- 4.修改`package.json`
- 5.安装`webpack`、`webpack-cli` (不推荐全局的安装`webpack`)
  - 全局安装
`npm install webpack webpack-cli -g` // 安装
`npm uninstall webpack webpack-cli -g`  // 卸载
  - 项目级安装
`npm install webpack webpack-cli   --save-dev`// 安装
`npx webpack -v` // 对版本进行打印


现在我们看一下我们配置好的`package.json `
```json
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "",
  "private": true,
  "author": "kim",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.41.2",
    "webpack-cli": "^3.3.9"
  }
}
```

#### webpack配置文件进行配置

- 我们想要对项目进行配置，我们先要编写一个配置文档，这个文档我们一般取名为 `webpack.config.js`
- 如果我们不想取名为上面的名字，如我们的配置文档叫做 `webpack.js`，那我们在执行打包的时候们就要指定一下配置文档的名字 `npx webpack --config webpack.js`
- 如果我们想要使用`npm script `进行执行代码的简化，那么我们需要在`package.json`进行配置`script`参数
- `webpack-cli` 我们安装这个是为了让我们在命令行中可以正确的运行`webpack`命令


json文件的配置script
```json
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "",
  "private": true,
  "scripts": {
    "bundle": "webpack"
  },
  "author": "kim",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.41.2",
    "webpack-cli": "^3.3.9"
  }
}
```
```javaScript
const path = require('path'); // node 中的内置模块路径

module.exports = {
  mode: 'development',
  entry: './src/index.js', // 打包的入口文件
  output: {
    filename: 'bindle.js', // 打包后输出的文件的名字
    path: path.resolve(__dirname, 'dist') // 绝对路径,__dirname是webpack.config.js所在的路径的位置,输出的文件放在bundle的文件夹下
  }
};
```

 
