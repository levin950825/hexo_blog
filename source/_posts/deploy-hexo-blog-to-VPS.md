---
title: Deploy Hexo Blog to VPS |部署Hexo博客到VPS
date: 2017-03-20 17:41:10
tags: [website dev, hexo]
categories: Hexo
comments:
toc: true
thumbnail:
banner:
---

Deploy my Hexo blog to VPS (Ubuntu OS at Digital Ocean)

<!-- more -->

## 前言

在本地搭建好Hexo博客后，接下来就是部署到互联网上去了。网上搜到的教程大部分都是将Hexo部署到GitHub Pages上面，这样省时省力，但是已经有了Digital Ocean的VPS还是要用一下。(当然了，GitHub上有空间限制，访问速度问题等等)。

运行环境：
> Ubuntu 16.04.1 x64
nginx 服务器

## 准备阶段

网上流传的武功秘籍分为两种：

- 将Hexo项目上传到VPS上面后执行 `hexo server`，之后配置Nginx反向代理，让域名指向 `http://localhost:4000`。
- 将Hexo在本地通过 `hexo generate` 生成静态文件，在通过 `hexo deploy` 部署到VPS上面，使用Nginx直接做Web服务器。

相比第二种方式，第一种每次写博客与更新博客时候的操作会很繁琐。所以我们使用第二种方式进行部署，这样既可以将静态文件deploy到VPS上，也可以上传到Github上用作备份，操作性和安全性上都要胜于前者。

而对于第二种方式而言，常用的又有 `git hook` 和 `rsync` 两种自动部署解决方案

## 当我们说部署Hexo博客的时候我们在讲什么
在我们创建的博客目录下执行 `hexo generate` 后，hexo会将我们编辑的markdown博客自动生成静态的网页，而生成的文件就存储在 `public` 文件夹中，这其中的每一个html文件都是我们之后在网页中查看博客时候加载的对应文件，而我们在执行 `hexo deploy` 时，就是要将 `public` 文件夹下的文件全部部署到我们之前在Nginx配置中设置的 `root /var/www/hexo` 的路径中。

## 用rsync部署
相比较git hook方式，这一种操作更简单，对于小白更不容易出错。

### 安装rsync

rsync的安装分为两部分：服务器端和本地
服务器端安装：

```bash
apt-get install rsync
```
> 对于Digital Ocean Ubuntu VPS，大多数的Linux系统已经默认装有 `rsync` 了，可以运行 `rsync` 测试下是否已经安装。
本地安装：

```bash
npm install hexo-deployer-rsync --save
```
### rsync配置

编辑博客文件夹目录下的 `_config.yml`，找到deploy添加如下代码：

```tml
deploy:
  type: rsync
  host: vps-ip  # 这里填写你VPS的IP地址，比如：138.23.23.23
  user: vps-user # 这里填写你登陆VPS所用的用户名，比如：root
  root: /usr/www/blog # 这里填写你在nginx中配置的文件路径
  port: 22 # SSH默认端口号，不需要修改
```
配置完毕！在 blog 目录下执行下面一段代码，接下来就是见证奇迹的时刻

```
hexo generate && hexo deploy
```


## git hook自动部署方案

使用git hook和rsync得到的效果是等效的，也就是说，二者中选一种你喜欢的就可以完成配置。

### 安装git
本地部署工具安装

```
npm install hexo-deployer-git --save
```

服务器端安装：

```bash
sudo apt-get update
sudo apt-get install git-core 
```
新增一个名为 `git` 的用户，并给用户 `git` 赋予无需密码操作的权限（否则到后面 Hexo 部署的时候会提示无权限）

```bash
adduser git
chmod 740 /etc/sudoers
vi /etc/sudoers
```
在vi编辑中找到如下内容：

```
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
```
在下面添加一行

```
git   ALL=(ALL)     ALL
```
> vim 操作小贴士：打开文件之后要按i进入编辑模式，编辑完以后按Esc，再输入`:wq`回车才能保存；如果想不保存直接退出请输入`:q!`。）
保存退出后执行

```
chmod 440 /etc/sudoers
```
接下来要把本地的 SSH 公钥上传到 VPS 。执行：

```
su git
cd ~
mkdir .ssh && cd .ssh
touch authorized_keys
vi authorized_keys
```
现在要打开本地的 Git Bash，输入`vi ~/.ssh/id_rsa.pub`，把里面的内容复制下来粘贴到上面打开的文件里。

然后建立放部署的网页的 Git 库。

```
cd ~
mkdir hexo.git && cd hexo.git
git init --bare
```
测试一下，如果在 Git Bash 中输入 `ssh git@VPS的IP地址` 能够远程登录的话，则表示设置成功了。

如果不成功，并且你的 VPS 的 ssh 端口不是 22 的话，请在Git Bash执行vi ~/.ssh/config，输入以下内容并保存：（成功就跳过这一步）

```
Host #VPS 的 IP
HostName #VPS 的 IP
User git
Port #SSH 端口
IdentityFile ~/.ssh/id_rsa
```

### 初始化git仓库
```
mkdir /var/repo
cd /var/repo
git init --bare blog.git
```
执行上述代码后,我们会在 `/var/repo` 路径下创建了一个名为 `blog.git` 的裸仓库，这个仓库的功能就是将我们deploy的文件通过git hook的方式共享到 `/var/www/blog` 中，而要想实现这一功能我们还需要进行如下配置：

在 `blog.git/hooks` 目录下新建一个 `post-receive` 文件：

```
cd /var/repo/blog.git/hooks
vim post-receive
```
在 `post-receive` 中添加如下内容：

```
#!/bin/sh
git --work-tree=/var/www/hexo --git-dir=/var/repo/blog.git checkout -f
```

设置 `post-receive` 文件的可执行权限：

```bash
chmod +x post-receive
```
提示：如果设置的路径文件夹并不存在，那么需要创建该文件夹并且赋予你所使用的用户的权限：

```bash
sudo mkdir /var/www/hexo
cd /var/www
chown git:git hexo
```
### 本地配置

编辑博客文件夹目录下的 `_config.yml`，找到deploy添加如下代码：

```
deploy:
    type: git
    repo: root@atomlx.com:/var/repo/blog.git # 此部分修改为你自己的登陆账号和域名，冒号后面为设置的裸仓库的地址
    branch: master
```
同样在 blog 目录下执行下面一段代码，我们也可以看到我们的文件部署到了服务器上

```
hexo generate && hexo deploy
```

## 最后
作为小白看到对 git hook 的教程之前有些出入，就选择了 rsync 的部署方法。Git hook 的流程只是放在这里作为对以后的参考。有什么不对的地方请谅解哦。


## 参考链接
* [Hexo快速搭建静态博客并实现远程VPS自动部署](https://segmentfault.com/a/1190000006745478#articleHeader0)
* [Hexo个人博客迁移至VPS并绑定独立域名](https://imp1995.github.io/2016/11/26/Hexo%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E8%BF%81%E7%A7%BB%E8%87%B3VPS%E5%B9%B6%E7%BB%91%E5%AE%9A%E7%8B%AC%E7%AB%8B%E5%9F%9F%E5%90%8D/)
* [如何部署Hexo到个人VPS](http://atomlx.com/2016/08/12/test/)
* [利用 GIT HOOKS 部署 HEXO 到 VPS](https://munen.cc/tech/Hexo-in-VPS.html)




