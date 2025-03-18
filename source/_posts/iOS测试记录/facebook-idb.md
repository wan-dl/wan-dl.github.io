---
title: facebook idb工具介绍
date: 2025-4-21
categories: iOS
icon: apple
---

## 前言

- idb Github仓库地址: [https://github.com/facebook/idb](https://github.com/facebook/idb)
- 官方文档：[https://fbidb.io/](https://fbidb.io/)


## idb companion
Each target (simulator/device) will have a companion process attached allowing idb to communicate remotely.

```
brew tap facebook/fb
brew install idb-companion
```

## idb client
A cli tool and python client is provided to interact with idb.

> idb 客户端需要安装 python 3.6 或更高版本。

```
pip install fb-idb
```

## 显示模拟器中安装的所有应用程序

```
idb list-apps 

idb list-apps --udid <udid>
```

## 安装错误

在安装idb过程中，会要求xcode版本。如下图:

![](/images/brew-idb-install-error-1.jpg)

根据提示，升级xcode版本即可。