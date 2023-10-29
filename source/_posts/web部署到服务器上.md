---
title: web部署到服务器
categories: 项目
date: 2023-10-30 19:00:00
updated: 2023-10-30 19:00:00
tag:
  - 前端
  - 项目
toc: true
thumbnail: "/images/color.jpg"
---
我们现在要将一个前端项目部署到阿里云的服务器上面，我们会从申请一个服务器然后部署这个逻辑进行实践
<!--more-->
## 服务器申请
我作为一个新用户，可以申请3个月免费的试用服务器，我们先通过这个途径进行举例
<img src='https://kimqueen.github.io/images/project/apply.png'/>
申请后我们会进入到如下的页面
<img src='https://kimqueen.github.io/images/project/connect.png'/>
我们进行远程连接，然后可以看到如下的页面
<img src='https://kimqueen.github.io/images/project/cmd.png'/>

我们进入控制台之后我们检查一下`node`的版本
``` cmd
node -v
```
我们发现服务器上并没有node，所以这个时候我们需要安装一个`node`,我们觉得使用`nvm进`行`node` 版本的管理更加符合预期,安装我们使用如下的命令
``` cmd
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```
安装成功之后，我们需要重新执行一下配置文件，让`nvm`的命令生效
```cmd
source ~/.bashrc
```
然后我们使用`nvm` 安装一下`node`
```
nvm install node
```
然后我们查看一下我们的`node` 版本，发现我们的node版本是`v21.1.0`
然后让我们来试一下node
``` cmd
  node
```
然后执行一下 
``` js
console.log('hello kim')
```
发现逻辑上面没有任何问题，截止到上面我们发现node 在服务器上面的环境已经是没有任何的问题了