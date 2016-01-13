---
layout:     post
title:      "关于git rebase的使用"
subtitle:   "usage of git rebase"
author:     "Jayn"
header-img: "img/about-bg.jpg"
tags:
  - git
---

之前有看过`rebase`的使用，但是当时感觉看的迷迷糊糊的，后来也一直没有用，现在来总结一下`git rebase`命令的使用。

### commit合并

当我进行了多次提交，然后感觉有些提交比较混乱，我想把最近的几次提交合并到一起，就可以使用`git rebase`命令.

假设我们将下面的4个最近的commit合并为一个：

>fb32838 highlight 
>46e201a change the pygments to rouge
>2b628ec highlight rouge 
>1d57607 fix highlight

首先我们使用如下命令：

>git rebase --interactive HEAD~4

执行后命令窗会出现如下的内容，可以编辑：

>pick 31f5476 Fixing bug #84. 
>pick da52aaa Fix bug #84. 
>pick 2750b94 Finally fix bug #84.

修改为如下的内容：

>pick   fb32838 highlight 
>squash 46e201a change the pygments to rouge
>squash 2b628ec highlight rouge 
>squash 1d57607 fix highlight

pick(可以用p代替)示要保留这个commit，squash(可以用s代替)表示要将这个commit合并到其他的commit上。保存退出后，会提示你输入合并后的新的commit message：

![gitRebase4](http://7xniym.com1.z0.glb.clouddn.com/gitRebase4.png)

带有`#`的是注释，不用管，灰色信息就是最后的合并后的commit信息，修改这些即可。修改后保存退出编辑器即可。
