---
title:  git的使用
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
我们主分支上面提交代码，我们使用git commit的命令进行提交，很简单，我在进行详细的描述。
<!--more-->
#### 分支
**创建分支**
我们通过实际操作看一下分支是什么样子的，比如说我们先要创建一个名字为newImage的分支。创建分支前：

![创建分支前](https://upload-images.jianshu.io/upload_images/13681871-3882a4bb98ef40fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用命令 `git branch newImage`之后

![创建分支后](https://upload-images.jianshu.io/upload_images/13681871-c7be916b54f6ec9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**在新的分支上面提交内容**
当我们向新的分支上面提交一些新的东西，如果我们直接执行`git commit`。

![直接执行git commit 命令](https://upload-images.jianshu.io/upload_images/13681871-ce3d4fd3df1b0bd0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们会发现master分支前进了，但是newImage分支还是待在原地。我们仔细看一下上图，我们发现现在在master上面有一个星号，这表示的是我们现在在master分支上面。所以我们正确的操作是，先切换到新分支上面，然后进行提交代码，我们执行的命令是:
`git checkout newImage ; git commit`

![git checkout newImage ; git commit命令执行之后](https://upload-images.jianshu.io/upload_images/13681871-69b02c066a494be7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**分支与合并**

***merge***
假如我们现在有两个分支，每一个分支上面都有一个独立的提交记录。这就意味着没有一个分支包含我们所有的修改的内容，我们要将这两个分支进行合并，现在我们看一下分支的情况：

![分支的合并](https://upload-images.jianshu.io/upload_images/13681871-4c6fd1e1ba9f4d1b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在我们将bugFix合并到master分支上面，使用命令：`git merge bugFix`

![合并之后的样式](https://upload-images.jianshu.io/upload_images/13681871-af12d445a0baeef0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是我们希望将master 分支合并到bugFix上面，我们需要先进行分支的切换，然后在进行合并，具体的命令如：`git checkout bugFix; git merge master`

![git checkout bugFix; git merge master执行之后](https://upload-images.jianshu.io/upload_images/13681871-852630cfe617e754.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面这样执行的是因为master继承自bugFix，多以Git什么都不需要做，知识简单的把bugFix移动到master指向的那个提交记录就可以了。
***rebase***
我们要是想将bugFix分支直接移到master分支上面，移动以后会让两个分支的功能看起来像按顺序开发的，但是实际上他们是并行开发的

![分支的合并前](https://upload-images.jianshu.io/upload_images/13681871-2a2d9986cc754454.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们执行`git rebase master`，现在bugFix的分支上面的工作在master分支的最顶端，我们看一下C3依然存在，而C3'是我们Rebase到master分支上的C3副本，现在的问题是master还没有更新，我们线面进行master分支的更新。

![分支合并后](https://upload-images.jianshu.io/upload_images/13681871-a84ccb0c7b8b9e62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在我们先切换到master分支上面，把它rebase到bugFix上面，合并的命令：`git rebase bugFix`

![更新master](https://upload-images.jianshu.io/upload_images/13681871-71011232f1b834a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 在提交树上移动
**HEAD**
HEAD是一个对当前检出记录的符号引用-- 也就是指向我们正在这个基础上进行工作的提交记录。
HEAD总是指向当前分支最近一次的提交情况。HEAD通常情况下指向分支名，在提交的时候，改变了分支的状态，这一改变导致HEAD是可见的。

![提交前](https://upload-images.jianshu.io/upload_images/13681871-e77e71d9d0dd7bf1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行命令：`git checkout C1; git checkout master ;git commit; git checkout c2`

![执行命令后](https://upload-images.jianshu.io/upload_images/13681871-2a04a65b3a7b15de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

***分离的HEAD***
分离的HEAD就是让其指向了某一个具体的提交记录，而不再是分支名
我们看一下现在的情况是：HEAD指向master，master指向c1

![HEAD的指向](https://upload-images.jianshu.io/upload_images/13681871-0708e80cbdede4e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果我们使用指令`git checkout C1`

![HEAD的指向发生了改变](https://upload-images.jianshu.io/upload_images/13681871-225708f7a07cbe1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在的执行变成了：HEAD指向C1
**相对引用**
- 我们可以使用^向上移动一个提交记录
- 使用^<num>向移动多个提交记录

![相对引用的具体应用](https://upload-images.jianshu.io/upload_images/13681871-eeeaf9283059f799.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以看一下，根据上面的意思也就是master^ 就是master的父节点。
所以我们执行命令`git checkout master ^`

![HEAD的位置发生了变化](https://upload-images.jianshu.io/upload_images/13681871-f136710e4cd47a90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们也可以将HEAD作为相对引用的参照，我们现在就举个栗子：
执行命令：`git checkout C3; git checkout HEAD ^; git checkout HEAD ^; git checkout HEAD ^`
执行命令前：

![before](https://upload-images.jianshu.io/upload_images/13681871-38823dfe0ec014e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行命令后：

![after1](https://upload-images.jianshu.io/upload_images/13681871-38d59c79612692c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

依次上移，直到：

![finally](https://upload-images.jianshu.io/upload_images/13681871-2e309d6146636649.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### ~操作符
如果在提交树上向上移动很多步骤的话，要敲很多的^,这样的就很麻烦，Git也考虑到了这一点，于是又引入先操作符~。
这个操作符后面可以和一个数字(可选，不跟数字时与^相同，向上移动一次)，指定向上移动多少次。
后退之前：

![一次性后退四次](https://upload-images.jianshu.io/upload_images/13681871-8f71f4b21f108af0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行：`git checkout HEAD~4`

![后退四次之后](https://upload-images.jianshu.io/upload_images/13681871-98e5af009ff0ca2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我使用相对引用最多的就是移动分支，可以直接使用-f选项让分支指向另一个提交。

`git branch -f master HEAD^3`:会将master分支强制指向HEAD的第3级父提交。

执行前：

![执行命令之前](https://upload-images.jianshu.io/upload_images/13681871-d114c8f70c21f8c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行后：

![执行命令之后](https://upload-images.jianshu.io/upload_images/13681871-642f0a4f4866cb1b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)











