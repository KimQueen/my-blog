---
title: webpack-生产环境
categories: webpack
tag:
  - 前端
  - webpack
  - JavaScript
  - 实践
toc: true
thumbnail: "/images/lie.jpg"
---
我们对生产环境和开发环境进行了略微的区分，但是要遵循不重复的原则，所以需要保留一个`common`的配置，为了讲这些配置合并在一起，需要使用一个工具`webpack-merge`,此工具会引用 `common"`配置，因此我们不必再在环境特定(`environment-specific`)的配置中编写重复代码。
#### 安装
```
npm install --save-dev webpack-merge
```
<!--more-->
将项目目录修改为如下的结构：

![项目目录](https://upload-images.jianshu.io/upload_images/13681871-377c71ae3b8afc2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

目录中的具体内容：

![webpack.common.js](https://upload-images.jianshu.io/upload_images/13681871-b9fdf17870a9be9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![webpack.prod.js](https://upload-images.jianshu.io/upload_images/13681871-fad7647731d6e3fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![webpack.dev.js](https://upload-images.jianshu.io/upload_images/13681871-788d5bd076a49dfc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在 `webpack.common.js` 中，设置了入口和出口的配置，并且在其中引入了二者都需要引入的插件。我们在`webpack.dev.js` 中，我们将 `mode` 设置为 `development`，并且为此环境添加了推荐的 `devtool`（强大的 source map）和简单的 `devServer` 配置。最后，在 `webpack.prod.js` 中，我们将 `mode`设置为 `production`，其中会引入之前在 tree shaking 指南中介绍过的 `TerserPlugin`。

在环境特定的配置中使用` merge() `功能，可以很方便地引用 `dev` 和` prod` 中公用的 `common` 配置。`webpack-merge` 工具提供了各种 `merge`(合并) 高级功能，但是在我们的用例中，无需用到这些功能。
#### npm scripts
把 `scripts` 重新指向到新配置。让 `npm start script `中 `webpack-dev-server` 使用 `development`(开发环境) 配置文件，而让 `npm run build script` 使用 `production`(生产环境) 配置文件

![package.json](https://upload-images.jianshu.io/upload_images/13681871-6a30fcf97e0b9e37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


