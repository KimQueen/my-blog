---
title: webpack-treeShaking
categories: webpack
tag:
  - 前端
  - webpack
  - JavaScript
  - 实践
toc: true
thumbnail: "/images/tree-2.jpg"
---
#### 添加一个通用模块
假设我们在项目中添加一个新的通用模块的文件`math.js`，并且导出了两个函数：
<!--more-->

![项目目录](https://upload-images.jianshu.io/upload_images/13681871-1442db7c994c0683.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![math.js](https://upload-images.jianshu.io/upload_images/13681871-07c08e91cdf18eaa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们将模式设置为`development`来确定`bundle` 是未压缩版本。

![webpack.config.js](https://upload-images.jianshu.io/upload_images/13681871-3b318e09d71f3ab6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后对现在的`index.js`的文档进行修改：

![index.js](https://upload-images.jianshu.io/upload_images/13681871-47fe9f15a89fbd82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意，我们没有从 `src/math.js `模块中 `import `另外一个 `square `方法。这个函数就是所谓的“未引用代码(dead code)”，也就是说，应该删除掉未被引用的` export`。现在运行` npm script npm run build`，并查看输出的` bundle`：

![bundle.js](https://upload-images.jianshu.io/upload_images/13681871-8a009d416410b926.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面的没有使用的`square `方法虽然没有使用过，但是他还是被打在了包里面。

#### 将文件标记为 side-effect-free(无副作用)

```
{
  "name": "your-project",
  "sideEffects": [
    "./src/some-side-effectful-file.js"
  ]
}
```
 "./src/some-side-effectful-file.js"这个文件将会被自动的打到保重，进行优化的时候，不会将它删除。


