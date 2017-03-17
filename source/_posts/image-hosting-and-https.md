---
title: Image Hosting and HTTPS|图床与HTTPS
date: 2017-03-17 15:53:45
tags: website dev
categories: Website Dev
comments:
toc: true
thumbnail:
banner:
---
Use images hosting service with HTTPS connections.
把图片放到支持HTTPS链接的图床平台上。

<!-- more -->
## 前言：
前几天把网站改到用HTTPS安全链接。
![](https://i.niupic.com/images/2017/03/17/jTJp2c.jpg)
![](https://i.niupic.com/images/2017/03/17/j5dYqp.jpg)

又想到一个任务是把博客的图片从Hexo `asset folder`加载模式改到用图床模式。这样一来有两个好处：

- 博客文章越来越多的时候hexo文件数会减半。因为之前的方式会给每篇post新建一个folder来放所有的图片等媒体
- 不会占用Digital Ocean的空间。购买的贫农级服务空间有限，能少用就少用点吧。

哦当然，要是用免费的图床服务啊。。有可能图床提供商整个服务崩掉。。那就没有然后了。。

#### Https 与小绿锁

按照前一篇笔记的流程给网站加了证书之后，浏览器中并不一定会显示可爱的小绿锁。为什么？因为网站可以还引用了别的 HTTP 资源，比如 js、css、图片、等等。必须全站资源均是 HTTPS，才会在浏览器地址显示小绿锁。（我当时主页面用`http://fonts...`
来引用Google Fonts，就不行。在文件里把链接改为`://fonts...`之后就好了。）

#### 图床选择
所以之前看到很多网上攻略用七牛的图床，但是经过多看多查，发现七牛的免费流量只支持HTTP，而HTTPS流量是要付费的。
来搜集一下支持HTTPS的免费图床：

- [imgur](https://imgur.com/)
- [sm.ms](https://sm.ms/) （5MB limit）
- [postimage.io](https://postimage.io/)
- [牛图网](https://www.niupic.com/) （无需注册）


