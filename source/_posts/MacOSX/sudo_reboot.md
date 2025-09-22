---
title: 设置macOS电脑sudo reboot免密
date: 2025-9-11
categories: MacOSX
icon: apple
---

## 设置macOS电脑sudo reboot免密

用 visudo 打开 sudoers 文件：

```
sudo visudo
```

找到文件结尾，加上一行（假设你的用户名是 apple）：

```
apple ALL=(ALL) NOPASSWD: /sbin/reboot
```