---
title: 02 - 学会查找帮助
date: 2024-5-27
categories: Vi/Vim
icon: vim
---

博客迁移，迁移前原文: [https://www.cnblogs.com/smstars/archive/2013/05/18/3085354.html](https://www.cnblogs.com/smstars/archive/2013/05/18/3085354.html)

##帮助
学习vi，学会使用联机手册或帮助命令是非常重要的。Unix有两个最重要的文档资料系统：Unix手册和Info（GNU项目的官方文档资料系统）。

常用常见命令：

　　1      man vi   

　　2      vi --help

　　3      info vi

对于较长的说明页，可使用分页程序分页显示。常用的分页程序有：less、more、pg。例如：man cp | less

查找说明书页的其它方法（基于web）：

1　　使用google搜索：（一定要确保包含双引号）

○    “man vi ”

○    “man pages” vi

2　　另外一种基于web的说明书页的方法是xman，xman是一个基于GUI的程序，它充当说明书页浏览器。在命令行启动xman：xman&。

## vi-vim帮助
如果还没接触vim，可在shell中输入vimtutor，查看教程。

vi-vim的帮助命令：

```
:help
```

## 一些其他　　
英文不好的同学，可在http://sourceforge.net/projects/vimcdoc/files/vimcdoc/1.5.0/vimcdoc-1.5.0.tar.gz/download?use_mirror=jaist，下载vim中文帮助。

使用中文帮助文档：

```
set helplang=cn
```

使用英文帮助文档：

```
set helplang=en
```