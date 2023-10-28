---
title: webpack学习之sourceMap
categories: webPack
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - webPack
toc: true
thumbnail: "/images/summary1.jpg"
---

#### sourceMap
在开发者模式下，默认`source-map`已经开启，如果我们想要关掉的话
```js
module.exports ={
  mode:'development',
  devtool:'none'
}
```

<!--more-->
如果我们期望以指定的方式开启，我们可以进行下面的配置：
```js
module.exports ={
  mode:'development',
  devtool:'source-map'
}
```
`source-map`:当有错误的时候，会生成一个映射关系，当我们没有开启`source-map`的时候，一旦代码出现问题，在控制台的错误提示，会提示我们打包后的js文件哪一行有错误，不利于我们进行错误的排查，我们开启`source-map`，`source-map`会为我们进行一次映射，体现在，我们在控制台查看错误的时候，会提示源代码中的哪一行有问题。
`source-map`:会在打包的路径下，出现一个`.map`文件，用来记住映射
`inline-source-map`:不会出现`.map`文件，这个映射直接以`base64`的形式记录到打包的目标文件中
`cheap-inline-source-map`:当项目很大，有`cheap`这个前缀就会大大的优化性能，对于保存，只会提示到行，不会再精确到列了，而且，按照这样的方式进行映射，只会针对自己的逻辑代码，不会再考虑第三方的模块。
`cheap-module-inline-source-map`：不仅仅针对自己的逻辑代码，还会考虑第三方的模块
`eval`:针对大型项目，可能提示信息不会很全

#### 最佳时间
开发环境
```js
module.exports ={
  mode:'development',
  devtool:'cheap-module-eval-source-map'
}
```
生产环境
```js
module.exports ={
  mode:'production',
  devtool:'cheap-module-source-map'
}
```
