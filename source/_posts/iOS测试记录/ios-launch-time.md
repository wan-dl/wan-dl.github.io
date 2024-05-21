---
title: iOS启动时间测试调研记录
date: 2025-5-21
categories: iOS记录
---

## 1. Xcode 配置DYLD环境变量

>注意：DYLD_PRINT_STATISTICS 仅适用于iOS 15以下。iOS以上不起作用。

DYLD 是动态链接器，可在执行应用程序时加载和链接应用程序的共享库。

- DYLD_PRINT_STATISTICS：录有关应用程序启动过程的统计信息，例如，应用程序启动完成后加载了多少镜像
- DYLD_PRINT_STATISTICS_DETAILS：记录动态加载程序何时调用构造函数程序和析构函数

![](images/xcode-edit-schema.jpg)

实际效果:

![](images/xcode-console-time.jpg)

## 2. 使用Xcode Instruments工具

Instrument是 苹果官方IDE Xcode 自带的 调试工具。
访问入口:Xcode 顶部菜单 【Xcode】->【Open Developer tool】->【lnstruments】。

![](images/xcode-menus-toos.jpg)

如下图所示，我们研究启动时间，主要用到两个。

- App Launch：用于查看App的启动过程，从而可以针对性的对启动速度进行优化
- Time Profiler：时间分析器，对运行在系统cpu上的进程执行基于低开销时间的采样

![](images/xcode-instruments.jpg)