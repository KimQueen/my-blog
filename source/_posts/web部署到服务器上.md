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

我作为一个新用户，可以申请 3 个月免费的试用服务器，我们先通过这个途径进行举例
<img src='https://kimqueen.github.io/images/project/apply.png'/>
申请后我们会进入到如下的页面
<img src='https://kimqueen.github.io/images/project/connect.png'/>
我们进行远程连接，然后可以看到如下的页面
<img src='https://kimqueen.github.io/images/project/cmd.png'/>

我们进入控制台之后我们检查一下`node`的版本

```cmd
node -v
```

我们发现服务器上并没有 node，所以这个时候我们需要安装一个`node`,我们觉得使用`nvm进`行`node` 版本的管理更加符合预期,安装我们使用如下的命令

```cmd
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

然后我们查看一下我们的`node` 版本，发现我们的 node 版本是`v21.1.0`
然后让我们来试一下 node

```cmd
  node
```

然后执行一下

```js
console.log("hello kim");
```

发现逻辑上面没有任何问题，截止到上面我们发现 node 在服务器上面的环境已经是没有任何的问题了

我们写一个简单的项目，可以运行的，然后将他达成一个压缩包如:`dist.zip`
然后我们在本地的路径下找到这个文件，确保我们的电脑有 ssh 的命令,使用命令

```
scp -r ./dist.zip  root@XXXX:/root
```

其中 XXXX 为公网服务器的地址，回车后会要求输入密码，我传输到 root 的目录下

我们回到服务器上，通过 ll 命令发现已经有了 dist.zip 文件了，这个时候我们需要通过 unzip 对其进行解压

```cmd
 unzip dist.zip
```

使用 ngix 进行部署

```cmd
yum install openssl
yum install zlib
yum install pcre
```

使用 yum 安装 nginx 需要包括 Nginx 的库，安装 Nginx 的库

```cmd
rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

使用下面命令安装 nginx

```cmd
yum install nginx
```

使用命令启动 nginx

```cmd
systemctl start nginx
```
这个时候我们查看自己的公网就可以看到 ip 地址了
修改文件的命令如下
```
 vim  /etc/nginx/nginx.conf
```
当我们发现我们的部署结果是 403 的时候，我们可以考虑是自己的 user 写错了，我改成自己的 user root

```
 user root;
```

其中里面的 server 的配置如下：

```cmd
    server {
       # listen       80 default_server;
       # listen       [::]:80 default_server;
         listen       80;
         server_name  8.140.198.156;
       # server_name  _;
       # root         /usr/share/nginx/html;
         root         /root/dist/index.html;
       # Load configuration files for the default server block.
         include /etc/nginx/default.d/*.conf;

        location / {
          root   /root/dist;
          index  index.html index.htm;
          try_files $uri $uri/ @router;
          }
    }
```
然后我们需要跑一下如下命令，确定自己的config 文件没有改错
```
 nginx -t
```
然后我们重启nginx 服务，确保我们的配置已经变更生效
```
nginx -s reload
```
