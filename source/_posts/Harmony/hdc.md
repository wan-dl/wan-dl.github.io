---
title: Harmony hdc命令行工具
date: 2025-3-18
categories: Harmony
icon: Harmony
---

## 获取连接的设备信息

```
hdc list targets

# 获取设备uuid
hdc shell bm get --udid
```

## 获取指定进程内存

```
hdc -t <deviceID> shell SP_daemon -N 1 -PKG io.dcloud.uniappx -r
```

## 手机常亮

```
hdc -t <deviceID> shell power-shell setmode 602
```

## 唤醒点亮手机屏幕

```
hdc -t <deviceID> shell power-shell wakeup
```


## 获取设备已安装的应用列表

```
hdc -t <deviceID> shell bm dump -a
```