---
title: Hexo Icarus Theme Settings (Icarus主题设置)
date: 2017-03-10 23:34:33
tags: hexo
categories: Hexo
comments:
toc: true
thumbnail:
banner:
---

关于Hexo Icarus主题配置的详细介绍，和相关的一些小技巧。
<!--more -->

## 前言

此篇不介绍HEXO的搭建，只讲ICARUS的个性化配置，HEXO的配置在国内也有很多的教程。
教程主要从[这里（作者MoRan_Sky）](http://moransky.xyz/2017/01/13/HEXO%20-%20Icarus%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE/)和[这里（作者Lemon）](http://lemon23.me/2016/10/21/Hexo%E7%9A%84%E8%BF%87%E5%9D%91%E8%AE%B0/)来。

发布者已经把基本的配置步骤写在了wiki里，还包括FQA，如果有什么解决不了的问题也可以到[Icarus GitHub Wiki](https://github.com/ppoffice/hexo-theme-icarus/wiki)去提问，或看有没有和你同样问题的回答。


## ICARUS配置

#### 什么是ICARUS

- 一种很清爽的HEXO主题
- 安装了评论，分享框架，非常之方便

#### ICARUS安装

ICARUS主题包可以在[这里](https://github.com/ppoffice/hexo-theme-icarus)找到，但更多人是采用GIT来下载的，切换到HEXO根目录,然后打入：

```zsh
$ git clone https://github.com/ppoffice/hexo-theme-icarus.git themes/icarus
```

下载之后将根目录里的

```yml
theme: 你之前的主题
```

改成：

```yml
theme: icarus
```

接下来可以输入`hexo s`来在本机生成，然后在 https://localhost:4000 打开博客主页。

#### LOGO配置

在博客页面的左上角，有一个LOGO，因为我们没有更改，所有这是作者的LOGO。

先确定你的LOGO图片的后缀是.png，然后把你的LOGO图片重命名为logo.png，然后在你主题文件中的css/images/文件夹，将这张图片粘贴来替换之前的图片。

#### 用户信息配置

在主页的左边，这里显示的是博客作者的信息。

打开主题的_config文件，找到profile一栏，你可以自定义你的profile信息。

```yml _config.yml
profile:
    enabled: true #是否显示博客作者信息在左侧
    avatar: css/images/avatar.png #博客作者的头像，可以到相应的路径去替换
    gravatar: #如果这里写入邮箱的话，头像会重新加载
    author: PPOffice #作者名
    author_title: Web Developer & Designer #一句话描述自己
    location: Harbin, China #位置
    follow: https://github.com/ppoffice/ #关注我按钮点击去到的网址
```

如果想把`FOLLOW`按钮稍作修改，变成`联系我`，并且把链接变成发送邮件到自己的邮箱的话：

```yml
    follow: mailto:wulala@123.com #效果是直接到发送邮件的界面，把languages/zh_CN.yml中的follow信息改成了联系我
```
#### 标签，分类和关于

在最上方的菜单栏，我们点击”标签”，”分类”，”关于”都会显示无法找到文件，ICARUS主题很好，它为我们预设了这三个分类。

在主题的`_source`文件里，把里头的三个文件全部复制，然后粘贴到HEXO跟目录的source文件夹，再刷新一下网页，就可以看见啦。

#### 代码高亮

ICARUS主题预设了许多代码高亮的主题，具体的名称可以在主题目录的`source/css/_highlight`文件夹里找到。
把主题的`_config`文件里的highlight类型换成你喜欢的类型。

关于选择代码高亮语言的问题，请看[这里](http://www.tuicool.com/articles/aamiimI)。

#### 搜索

ICARUS主题提供了3种搜索方式，个人喜欢insight，当然你也可以选择baidu或者swiftype。

使用insight的话先把博客根目录下面的`_config.yml`里面的

```yml
search:
    insight: false
```
改成

```yml
search:
    insight: true
```
接下来需要安装一个小插件：

```zsh
npm install hexo-generator-json-content --save
```
接下来可以刷新一下网页，点击右上角的搜索框，就会发现已经成功配置了。

#### 删除搜索按钮

如果使用的是insight搜索，所以那个搜索按钮没有什么意义，可以找到主题文件中的`layout/search/index.ejs`，将里面的:

```html
<form class="search-form">
    <input type="text" class="ins-search-input search-form-input" placeholder="<%= __('index.search') %>" />
    <button type="submit" class="search-form-submit"></button>
</form>
```
改成：

```html
<form class="search-form">
    <input type="text" class="ins-search-input search-form-input" placeholder="<%= __('index.search') %>" />
    <!--button type="submit" class="search-form-submit"></button-->
</form>
```
之后你就会发现那个搜索按钮没了。

#### 多说评论

评论是个很重要的东西，这可以让你和读者进行沟通。

如果需要开启评论功能，需要先去相应的地方注册账号，然后按照提示把ID或名字添加上。点击[这里](https://github.com/ppoffice/hexo-theme-icarus/wiki/Comment) 跟着做就对了。剩下一些评论框自定义的样式就自行百度，或注册后在网站内进行更改设置就好了。

多说：在多说官网先创建一个账号，然后再[这里](http://duoshuo.com/create-site/)创建一个站点，站点地址写博客的地址，在多说域名那里按照要求填入，填入的名称（也就是去掉.duoshuo.com）就是多说的shortname。

ICARUS已经为你写好了评论的框架，你不需要自己搭建，你只需要把主题的_config文件中的comment改成这样：

```yml
comment:
    disqus: #写入你的disqus shortname, if have
    duoshuo: #写入你的多说shortname, if have
    youyan: #写入你的友言shortname, if have
```
之后刷新页面，拉到下面，就会看到评论的界面啦。你可以前往你自己的多说域名然后自定义评论。
对于想使用Disqus的用户，去Disqus官网新建个账户。Create a new forum， 然后按照官方指南把自己的forum integrate到博客来。现在对于Hexo平台还没有integrate好的选项，没关系，等到你选择自己的forum名字的时候记住那个shortname，到icarus主题里的`_config`文件里填上就好了。主题会帮你做好所有的integrate。



#### 把文章界面放宽

在ICARUS主题里，如果你把屏幕放的宽一点，真正的文章就只有中间一小条，我们需要稍微把文章DIV放宽一点。

把主题的`source/css/_variables.styl`文件打开，翻到43行左右，把：

```
main-column = 7
```
改成

```
main-column = 10 //或者也可以改成9，着看个人喜好，我觉得10正好合适
```

#### 分享

在本地使用bdshare，jiathis都是可以的，但在Github上神奇的消失了。我也试过bshare，dsshare，但还是神奇的消失了，读者可以自己的试一下。如果你也用Github Page，建议把主题的`_config`文件中的share改成:

```
share: addtoany
```

#### 友情链接

ICARUS自带友情链接功能，在主题的`_config`中的miscellaneous里：

```yml
links:
    显示名: 链接
```
比如：

```yml
links:
    GitHub: http://github.com
    MCBBS: http://mcbbs.net
```

#### 设置友情链接为打开新页面

之前的友情链接会在自身的博客覆盖，如果想要在新窗口打开（比较推荐这样）。

把主题文件的`layout/widget/links.ejs`打开，把：

```ejs
<li>
     <a href="<%- theme.miscellaneous.links[i] %>"><%= i %></a>
</li>
```
改成

```ejs
<li>
     <a href="<%- theme.miscellaneous.links[i] %>" target="_blank"><%= i %></a>
</li>
```

#### 社会链接

在关注我按钮的下面的下面，有一排图标，这就是社会链接，也就是你在别的地方的首页。它是fa fa-icon，ICARUS自带FontAwesome，所有你再安装的时候只要写入class名就可以。详细的图标内容可以在[这里](http://www.yeahzan.com/fa/facss.html)找到。

格式：`fa-后面的内容，图标: 链接`

打开主题的`_config`文件：

```yml
social_links:
     github: #你的Github首页
     envelope: #你的邮箱
     user: #博客主页
     reorder: #博客分类页面
     sort-amount-asc: #博客历程页面
```
在这里，我们只需要输入fa-后面的内容当图标就行了。

#### 自定义社会链接title

在社会链接的图标上停留的时候，会弹出图标名的title，但有时候我们想自定义title，怎么办呢？

打开主题文件中的`layout/common/profile.ejs`文件，把：

```ejs
<a href="<%- url_for(theme.customize.social_links[i]) %>"   target="_blank" title="<%= i %>" <%= tooltipClass %>>
    <i class="fa fa-<%= i %>"></i>
</a>
```

改成：

```ejs
<a href="<%- url_for(theme.customize.social_links[i].link) %>" target="_blank" title="<%= theme.customize.social_links[i].title %>" <%= tooltipClass %>>
    <i class="fa fa-<%= i %>"></i>
</a>
```
那么我们在config里的social_links的表达内容就是：

```yml
图标名: 
    link: 链接
    title: 鼠标选中的文本
```

#### 横幅和略缩图

在HEXO的根目录source/_posts文件夹，里面有你的文章，ICARUS允许你为文字设置横幅与略缩图，切记都要在两个—之间输入。
略缩图的设置方式：`thumbnail：图片`

横幅的设置方式：`banner：图片`


#### 更多
如果还想自定义CSS的话，主题样式文件都在`themes/icarus/source/css/_partial`里，对照着页面文件找到对应的class样式去修改吧。

比如我这里发现在页面宽度是。。。的时候，两栏的效果并不是很理想。
![](14891972108134.jpg)

时候可以在测试页面按`cmd+opt+j`调出Javascript console查看元素。会发现它是从根目录下`public/css/style.css`而来，但是我们直接改这里的话是没有用的。因为所有`public`文件夹下都是由`hexo g`生成的。

所以这里要更改我们所用主题的css生成文件。例如我的在这个路径下`~/hexo_blog/themes/icarus/source/css`

桑心的是，我试图修改了`_variables.styl`里面的sidebar-column之类，然后运行

```bash
$ hexo clean
INFO  Deleted database.
INFO  Deleted public folder.
$ hexo g
$ hexo s
```
发现并没有什么*用。。。 新手小白在这里欢迎指教啊~
