---
title: xcodebuild命令行工具
date: 2024-5-22
categories: iOS
icon: apple
---

通常，我们都是在Xcode界面中进行操作。如果我们想在命令行操作，这个时候就会用到xcodebuild.

xcodebuild是由Xcode提供的一种命令行工具，它允许开发者在终端中执行Xcode的一些操作。比如：

- 工程编译
- 自动化测试

## 查看sdk版本

```bash
xcodebuild  -showsdks
```

## 项目信息

```bash
xcodebuild -list
```

命令行输出结果:
```accesslog
Command line invocation:
    /Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild -list

User defaults from command line:
    IDEPackageSupportUseBuiltinSCM = YES

Information about project "test":
    Targets:
        test
        testUITests

    Build Configurations:
        Debug
        Release

    If no build configuration is specified and -scheme is not passed then "Release" is used.

    Schemes:
        test
```

## 项目构建

```bash
xcodebuild -project ~/t-projects/test/test.xcodeproj -target test


xcodebuild -project ~/t-projects/test/test.xcodeproj -alltargets -configuration Release -sdk iphonesimulator -arch x86_64
xcodebuild -project ~/t-projects/test/test.xcodeproj -alltargets -configuration Release -sdk iphoneos -arch arm
```

注意事项：
1. -arch: 指的是CPU架构。比如x86_64、arm

## 安装应用到iOS模拟器

```bash
xcrun simctl install booted test.app
xcrun simctl install booted testUITests-Runner.app
```

## 安装应用到iPhone真机

```bash
ideviceinstaller -u 00008110-001C35240CF2801E -i test.app
```

## xctest

> 相关的文档: [https://www.cnblogs.com/wang-wang-blog/p/17127781.html](https://www.cnblogs.com/wang-wang-blog/p/17127781.html)

运行测试到iOS模拟器

```bash
xcodebuild test -scheme test -destination 'platform=iOS Simulator,id=692AFF1D-3B88-400D-A9AB-99E50FF61D50'
```

运行测试到iOS真机

```bash
xcodebuild test -scheme test -destination 'platform=iOS,name=wang'
xcodebuild test -scheme test -destination 'platform=iOS,id=00008110-001C35240CF2801E'
```