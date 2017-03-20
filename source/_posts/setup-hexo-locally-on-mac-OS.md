---
title: Setup Hexo Locally on Mac OS |Hexo在Mac的本地安装
date: 2017-03-02 17:54:17
tags:hexo
categories: Hexo
comments:
toc: true
thumbnail:
banner:
---

My memo for installing Hexo on my local computer (Mac OS: Sierra v 10.12.3)
<!-- more -->

### Make sure `Git` and `Node.js` is installed
### 创建网站目录
在任意位置创建一个文件夹，作为网站目录，并通过 cd 命令进入文件夹
`cd ~/Documents/Passion/mysite`
### 安装 Hexo
```bash
npm install -g hexo-cli
hexo init
npm install
hexo d -fg
hexo server
```
> When running the first command, I encountered error like `Error: Cannot find module '...`. If you face the same thing, try the following link to fix it. https://gist.github.com/DanHerbert/9520689

打开 http://localhost:4000 即可看到你的站点（当然还没有发布到网络）。
以后加了新的文章或者改动之后，直接`hexo server`就可以在4000端口检测了。

参考资料：
http://blog.fens.me/hexo-blog-github/

Hexo Themes:
https://github.com/letiantian/huno
https://www.zhihu.com/question/24422335

我喜欢的：
https://github.com/raytaylorlin/hexo-theme-raytaylorism
http://blog.zhangruipeng.me/hexo-theme-icarus/about/
http://keyin.me/

### 遇到的问题：
重启机器后hexo和npm都不见了，显示错误如下：
`zsh: command not found: npm`
`zsh: command not found: hexo`
执行命令 `ls ~/.npm` 发现执行文件都在此目录下

解决步骤：

**1. 卸载已经安装的 `node`, `npm`, 和 `nvm` (if exists)**

To completely uninstall node + npm is to do the following:

go to /usr/local/lib and delete any node and node_modules
go to /usr/local/include and delete any node and node_modules directory
if you installed with brew install node, then run brew uninstall node in your terminal
check your Home directory for any local or lib or include folders, and delete any node or node_modules from there
go to /usr/local/bin and delete any node executable
> http://refactor.ghost.io/2016/01/17/how-do-i-completely-uninstall-node-js-and-reinstall-from-beginning-mac-os-x/

`rm -rf $NVM_DIR ~/.npm ~/.bower`

**2. 在Terminal运行下列命令来安装`nvm`**
> `nvm` 是`Nodejs`版本管理器

Please check the official `nvm` docs for the latest version link.

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
```
安装之后请重启Terminal

**3. 安装切换各版本 node/npm**

```bash
# 查看有哪些版本可供安装：
nvm ls-remote
# 安装最新稳定版本
nvm install stable
# 这里再安装一个0.12.7版本 并把它设为默认
nvm install 0.12.7
nvm alias default 0.12.7 
```
**4. 使用 .nvmrc 文件配置项目所使用的 node 版本**
如果你的默认 node 版本（通过 nvm alias 命令设置的）与项目所需的版本不同，则可在项目根目录或其任意父级目录中创建 .nvmrc 文件，在文件中指定使用的 node 版本号，例如：

```bash
cd <项目根目录>  #进入项目根目录
echo 4 > .nvmrc #添加 .nvmrc 文件
nvm use #无需指定版本号，会自动使用 .nvmrc 文件中配置的版本
node -v #查看 node 是否切换为对应版本
```

其他人遇到的类似问题解决方法:

https://yrom.net/blog/2016/08/10/auto-load-node-on-zsh-startup/






