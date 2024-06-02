---
title: 15 - shell命令
date: 2024-06-01
categories: Vi/Vim
icon: vim
---

博客迁移，迁移前原文: [https://www.cnblogs.com/smstars/archive/2013/05/18/3085347.html](https://www.cnblogs.com/smstars/archive/2013/05/18/3085347.html)


## 执行shell命令

|命令			|说明	|
|--				|--		|
|:!command		|		|暂停vi，执行制定的shell命令	|
|:!! pause vi	|		|执行上一条shell命令			|
|:sh			|		|暂停vi，启动一个新的shell		|
|:!csh			|		|暂停vi，启动一个新的c-shell	|

## 使用shell命令处理数据

|命令			|说明	|
|--				|--		|
|n!! command	|		|对n行数据执行command							|
|!move command	|		|对当前光标至move所指定的位置的数据执行command	|
|!move fmt		|		|格式化当前光标到move所指定的行					|

