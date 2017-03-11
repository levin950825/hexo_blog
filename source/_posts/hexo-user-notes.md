---
title: Hexo User Notes|Hexo的基本使用
date: 2017-03-11 08:33:24
tags: hexo
categories: Hexo 
comments:
toc: true
thumbnail:
banner:
---

使用Hexo的常用命令和一些格式上的小技巧。

<!--more -->

#### 基本命令
1. `$ hexo new [layout] <title>` 创建一篇新文章。其中`layout`是可选参数，默认值是`post`。Hexo提供的`layout`在`scaffolds`目录下，也可以在此目录下自建`layout`文件。新建的文件则会保存到 `source/_post`目录下。你也可以更改默认布局的参数，如post布局，你需要打开 `scaffolds/post.md`，增加类别和描述。再新建一篇文章就能看到增加了文章参数。

2. `$ hexo generate` 也可简写为`$ hexo g` 在部署前需要通过该命令把所有的文章做静态化处理，生成相应的html、javascript、css。

3. `$ hexo deploy`也可简写为$ `hexo d` 负责生成静态文件后的发布。在发布到你的服务器之前要先配置好deploy指令，在全局配置文件(`_config.yml`)中找到deploy，并修改属性值。


#### 在文章中加入图片
要在文章中插入图片，有两个选择，一是使用图床，二是将图片与网页文件一起部署。两种方法各有利弊，如果打算部署到GitHub或GitCafe上，个人推荐使用图床。如果是部署到七牛上，则直接使用部署方式。


#### 不在主页显示全部文章内容
我们在编辑内容时，在合适的地方添加一行代码：`<!-- more -->`，在它之上是摘要，是在主页会显示的文字部分。在它之下是余下全文，在主页中会多一个阅读全文的按钮，点击后才会看到全部内容。

#### 文章基本参数
打开 `scaffolds/post.md` 增加类别和描述，修改默认布局的参数：

```markdown post.md
---
title: #标题
date: #日期时间
tags: #标签
categories: #分类
comments: #是否允许评论 true 或 false
toc: #是否添加文章目录 true 或 false
thumbnail: #侧边栏封面图
banner: #文章顶部封面图
---
```
在新建一篇文章，你就能看到新建的文章已经新增了这些参数，不用每次自己手打，不需要的值就可以空着。

#### 代码高亮并为代码块添加标题
Markdown中插入代码，是通过添加三个反引号（`）或三个波浪号（~）来实现的。如下示例

``` [language] 
code here 
```
像大多数的markdown, 如github的markdown都是这种写法。这种写法在[language]后面不能加其它参数，否则会输出不正常。

Hexo自带高亮：[Code Block](https://hexo.io/docs/tag-plugins.html#Code-Block)

使用格式如下：

```markdown
{% codeblock [title] [lang:language] [url] [link text] %}
code snippet
{% endcodeblock %}
```

注意：如果使用静态的代码高亮，则必须关闭hexo自带高亮，关闭之后，如果以前的.md源文件使用的是hexo第二种插入代码的方式，则会导致`hexo-renderer-marked`渲染异常。


#### 关于 Markdown 语法格式
尽量使用规范的 Markdown 语法格式
参考：http://www.cirosantilli.com/markdown-style-guide/#code

