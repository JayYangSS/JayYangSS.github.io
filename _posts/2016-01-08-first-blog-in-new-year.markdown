---
layout:     post
title:      "新年第一篇"
subtitle:   "jekyll blog writing"
author:     "Jayn"
header-img: "img/post-bg-04.jpg"
tags:
  - jekyll
  - blog
---

折腾了一下午，终于把这个博客搞起来了。真是纠结啊。先来记录一下搞这个东东踩到的一些坑吧。

首先遇到的一个问题，就是感觉jekyll系统虽然更行文章内容比较方便，实现了博客内容和样式的分离，但是感觉这个文件的规范还是比较严格。每一篇博客是一个markdown文件，这个文件的文件名不能是中文，如果是中文的灰，就找不到这个文件，看了一下，是中文URL乱码了。文章的名字当然可以起中文名字了，不过是在这个markdown文件的最开始指定的。即在文章的头信息中指定：`title:"文章名称"` 

接下来的一个坑，就是时间问题，发现我在文件的头信息中修改时间：`date: 2016-01-08`,然后就跪了，这篇文章在博客中中就不显示了。。。在经过了多次的尝试后，我终于发现，原来是这样的:

1. 如果在文件的头信息中不写时间信息的话，默认的时间是根据markdown文件名中的日期来决定的，如文件名为：*2016-01-08-first-blog-in-new-year.md*,则文章的创建时期为`January 8,2016`；
2. 若文件中的头信息提供了日期信息，那么文章的发布时间根据这个来确定，会覆盖文件名中包含的时间信息；
3. 尝试了一下，发现博客不显示的原因是由于文件头信息提供的时间超过了当前时间，所以就没有了。。。反正我经过测试发现是这样的，不知道是不是这个原因导致的。

还有一个问题，发现修改了一些内容，在本地启动运行，博客内容已经更新，但是推到github上后，没有变化，应该是github还没有build出新的版本，也有可能是本地浏览器缓存下来的内容，导致更新后的内容看不到，在URL后面加`?v=1`可以解决这个问题。

## 关于评论系统的添加

我使用的评论系统是“多说”，但是在设置的时候是有点egg pain,多说的账号设置我一开始绑定我的微博，我的微博账户是带汉字的，然后就跪了，怎么则么都不好使，后来将多说的用户名称修改为英文，然后就好使了。

关于jekyll的使用，还有很多特性还不太了解，以后一边用一边学吧。感觉用这个写来的Blog样式还是不错的。以后就在这边多多记录自己的学习笔记和一些感悟吧。多多总结，不断提高自我。

## 添加Google Analytics

Google Analytics是用来做用户访问统计的。在Google Analytics注册账号后，会得到Google统计代码，将这个放到自己的每一个网页上即可，具体做法是：

在` _includes/google_analytics.html`中将Google生成的代码贴进去，然后在`_layouts/default.html`中将如下代码添加：

>\{_%_ include google_analytics.html %\}  

测试了一下，然后就可以工作了啊。下面是简单的测试结果：

![google_analytics](http://7xniym.com1.z0.glb.clouddn.com/google_analytics.png)

## 代码高亮功能

在`_configure.yml`中要指明高亮使用的分析器，jekyll默认使用`pygments`，我将代码push到github上是可以高亮显示的，但是在本地无法运行jekyll，原因是在本地我的`pygments`总是安装失败，所以我在`_configure.yml`中换成了`highlighter: rouge`,虽然没有`pygments`支持的语言多，但是够我用就行了。这样代码就可以高亮显示了。`rouge`支持的语言在[这里](https://github.com/jneen/rouge/wiki/list-of-supported-languages-and-lexers).

在平常写markdown文章时，代码高亮很简单，就是将代码直接写在两组3个点号之间就可以了，这种写法在本地运行jekyll时没有问题，但是当push到github上去时就无法高亮显示了，而是要使用如下方式才可以高亮显示：

> \{_%_ highlight *词法分析器* %\}  
> 需要高亮的代码  
> \{_%_ endhighlight %\}

<a href="#">
    <img src="{{ site.baseurl }}/img/jekyll_github.png" alt="Post Sample Image">
</a>
<span class="caption text-muted">
