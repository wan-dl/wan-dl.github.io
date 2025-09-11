---
title: 解锁 Mac 电脑上的登录钥匙串
date: 2025-9-11
categories: MacOSX
icon: apple
---

### 解锁 Mac 电脑上的登录钥匙串（login.keychain）

解锁 Mac 电脑上的登录钥匙串（login.keychain），以便后续程序或脚本可以访问钥匙串中存储的密码、证书等敏感信息.

在脚本或自动化任务中，某些操作（如访问证书、签名应用、读取密码等）需要钥匙串处于解锁状态。

这个命令会用给定密码解锁当前用户的钥匙串，使后续命令可以无交互地访问钥匙串内容。

```shell
security unlock-keychain -p <USER_UNIVERSAL_PASSWORD> ~/Library/Keychains/login.keychain'
```