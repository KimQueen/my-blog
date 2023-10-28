---
title: webpack学习之loader
categories: webpack
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - webPack
toc: true
thumbnail: "/images/tree.jpg"
---
#### loader的作用
用来将`webpack`无法处理的东西。转换成`webpack`可以处理的东西，`webpack`理论上只可以处理`js`的语言。
-  `test:loader`用来处理哪些文件，这个部分一般使用正则来表示
- `use`：将满足的条件进行转换的时候，使用的是哪个loader

<!--more-->
```js
  module: {
    rules: [
      {
        test: /\.(jpg|png)$/, // 用正则标识对哪些文件进行处理
        use: {
            // 对于满足正则的文件使用什么loader，这个loader需要通过npm 进行安装
          loader: 'file-loader', 
          options: {
            // placeholder 占位符
            name: '[name].[ext]', // 以相同的文件名和后缀进行打包
            outputPath: 'images/'
          }
        }
      },
      {
           // 用正则标识对哪些文件进行处理,正则对满足.scss结尾的文件执行下面的loader
        test: /\.scss$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              importLoaders: 2
            }
          },
          'sass-loader',
          'postcss-loader'
        ] // loader 的执行顺序是从下到上，从右到左
      }
    ]
  },
```
*那么对于打包来说，究竟是什么样的流程呢？*
- `npm run bundle` // 或是npx webpack 执行这一命令
- 找到`webpack.config.js`配置项，根据配置项来进行打包，如果没有这个文件，会执行默认的参数进行打包
- 在打包的过程中，遇到`js`文件会直接进行打包，但是遇到其他类型的文件，如`jpg`文件，会查询配置文件，然后根据配置文件的参数，使用固定的`loader`进行打包
- 如遇到`jpg`，就会使用`file-loader`进行打包

*那么`file-loader`究竟做什么了？*
- 发现了引入了图片
- 将图移动到指定的目录下
- 移动后，获取到新的文件的名字
- 将名称返回给引入模块的变量之中

其实`url-loader`和`file-loader`功能很相似，就是多了一个`limit`参数，我们可以设置这个参数，来决定什么时候用`url-loader`

#### 样式的打包
流程：
- 引入了一个`css`文件
- 需要先要安装 `css-loader`和`style-loader`
- 遇到`css`文件就会去使用`css-loader`和`style-loader`进行打包
- 把样式文档放在指定的文件中

`css-loader`：分析几个`css`文件之间的关系，并将几个`css`文件合并成一个`css`文件中
`style-loade`r：将`css-loader`合并的`css`文件挂载在指定文件的header上面

我们如果配饰了`sass-loader`或是`less-loader`的话，我们一般需要配置`importLoaders：2`,因为执行顺序是从下到上，从右到左，所以一点引入的`less`文件里面再次引入`less`文件会导致，打包可能不在走之前的`sass-loader`, `postcss-loader`,我们配置`importLoaders：2`就是为了规避这个问题
