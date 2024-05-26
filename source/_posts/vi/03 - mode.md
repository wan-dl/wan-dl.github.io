---
title: 03 - 模式和命令
date: 2024-5-27
categories: Vi/Vim
icon: vim
---

博客迁移，迁移前原文: [https://www.cnblogs.com/smstars/archive/2013/05/18/3085352.html](https://www.cnblogs.com/smstars/archive/2013/05/18/3085352.html)
 
## 输入模式和命令模式
命令模式[2]（command mode）：所键入的键都被解释成命令。

输入模式（input mode）：键入的任何内容都直接插入到编辑缓冲区中。

当离开输入模式时，使用Esc键切换到命令模式。

了解所处模式的方法：

```
：set showmode
```

## vi和ex命令
vi和ex是同一个程序的两种不同的表现形式。也就是说可以同时使用vi和ex命令。

vi：

①大多数vi命令都是单字母或双字母的表现形式。

②vi键入时命令不回显。

ex：

①ex命令比vi命令长。

②所有的ex命令都以一个:（冒号）开头。键入:后，vi就将光标移动到命令行上（屏幕最底部）。

③ex命令的每个字符都将回显。

④ex命令结束，必须按下回车键。

## 命令补全
命令补全：`Tab`

关键字补全：`^N ^P`