---
title: xcrun notarytool 命令行工具 - 对文件进行公证审核
date: 2025-4-23
categories: iOS
icon: apple
---

Notarization(公证): 提交macOS 软件（或其它运行程序文件），到Apple检查审核。

此操作可以检查程序是否存在恶意代码。

## 如果不对软件进行Apple公证审核会怎么样？

下面我们将打开一个未经过Apple公证的软件。

![](/images/xcrun-notarytool-open-2.jpg)

## 打开已经过Apple公证审核的软件

![](/images/xcrun-notarytool-open-1.jpg)

## 如何公证？

生成App专用密码。文档参考: [https://support.apple.com/zh-cn/102654](https://support.apple.com/zh-cn/102654)

命令行提交指定文件

```bash
xcrun notarytool submit ~/xxxx.dmg --apple-id "<apple-id>"  --team-id "<team-id>" --password "<passwd>"

# 请正确填写appid、team-id、App专用密码
xcrun notarytool submit ~/xxxx.dmg --apple-id "appleid@xxxx.com"  --team-id "YQXXXXXXXX" --password "fajy-xxxx-xxxx-xxxx"
```

## 查看公证历史和公证日志

```bash
xcrun notarytool history --apple-id "<apple-id>"  --team-id "<team-id>"  --password "<passwd>"

xcrun notarytool log "xxxxxxxxxxxxxxxxxxxxxxxxxxxx" --apple-id "<apple-id>"  --team-id "<team-id>"  --password "<passwd>"
```