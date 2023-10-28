---
title: 多分支git的开发
categories: 工具
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - git
  - 工具
toc: true
thumbnail: "/images/too-1.jpg"
---
---
我们在进行代码管理的时候，经常要使用git，理论上会有一个`master`主分支，以及`release`的测试分支（用于版本的发布），`develop`的研发自己进行分开发的分支。我们一般的流程是在`develop`上进行开发，然后到要发版本的时候，将`develop`的分支合并到`release`分支上面，然后在`release`测试完成，完全没有问题的时候，会将`release`分支的代码合并到`master`的分支上面，将`master`分支的代码作为发版的代码发布出去。现在简单写几个我们需要用到的命令。

<!--more-->

![工作区与暂存区](https://upload-images.jianshu.io/upload_images/13681871-0bbc336026261632.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


`git checkout -b develope origin/develop` 将代码以不融合的方式拉取下来，并且直接切换到`develop`的分支上面
`git status` 检查在这个分支上面都有哪些代码进行修改了
`git add .` 将修改的文件提到本地的暂存区
`git commit -m 'kim:【other】xxx'` 实际上就是把暂存区的内容提交到了到了当前的分支 -m的意思是添加注释，后面是注释的内容
`git pull origin develop` 将远程的develop分支上的东西拉取下来并和本地的代码进行融合
`git push origin develop` 将本地的分支上面的东西推送到了远程仓库中

![stash的使用场景](https://upload-images.jianshu.io/upload_images/13681871-1c0b03c6596fc629.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`git stash` 对本地的一些不想提交的东西开辟了一写空间进行展示性的存储，这样方便我们切换到别的分支上进行工作

![工作目录就干净了](https://upload-images.jianshu.io/upload_images/13681871-6f5581cf39d1047d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`git stash list` 可以看一下我们stash的情况

![存储的情况](https://upload-images.jianshu.io/upload_images/13681871-eaef7cccfd9a5098.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果我们想要使用暂存区中的东西 ，我们可以使用git stash apply。举个栗子：`git stash apply stash@{2}`
你也可以运行 `git stash pop` 来重新应用储藏，同时立刻将其从堆栈中移走。



