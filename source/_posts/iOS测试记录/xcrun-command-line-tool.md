---
title: xcrun命令行工具
date: 2025-5-21
categories: iOS记录
---

### 查看iOS模拟器列表

```bash
xcrun simctl list devices
```

### 启动模拟器

```bash
xcrun simctl boot <UDID>

open -a simulator
```

### 关闭模拟器

```bash
xcrun simctl shutdown <UDID>
```

### 重置模拟器

```bash
xcrun simctl erase <UDID>
```

### 清理不可用的模拟器

> 当Mac空间不够用时，这条命令或许可以帮你重获不是磁盘空间。
```bash
xcrun simctl delete unavailable
```

### 切换主题模式

```bash
# 浅色主题
xcrun simctl ui <UDID> appearance dark

# 深色主题
xcrun simctl ui <UDID> appearance light
```

### 截图

> booted代表当前启动的模拟器；也可指定模拟器<UDID>

```bash
xcrun simctl io booted screenshot xxx.png

xcrun simctl io <UDID> screenshot xxx.png
```


### 录制模拟器视频

> 在终端按Ctrl+C来停止录屏.
```
xcrun simctl io booted recordVideo test.mp4
```

### 启动指定应用名称

```bash
xcrun simctl launch booted <packageName>

xcrun simctl launch <UDID> <packageName>
```

### 关闭已打开的应用

```
xcrun simctl terminate booted <packageName>

xcrun simctl terminate <UDID> <packageName>
```

### 设置或清除状态栏

```bash
// 设置时间
xcrun simctl status_bar booted override --time 00:00

// 设置电量为50
xcrun simctl status_bar booted override --batteryLevel 50

// 清除修改
xcrun simctl status_bar booted clear
```

### 隐私授权

```bash
xcrun simctl privacy booted grant all <packageName> 
```


### xcrun可用的命令

下面是xcrun --help的输出。

|命令				|	描述																																|
|--					|--																																	|
|addmedia			|   Add photos, live photos, videos, or contacts to the library of a device.														|
|boot				|   Boot a device or device pair.																									|
|clone				|   Clone an existing device.																										|
|create				|   Create a new device.																											|
|delete				|   Delete specified devices, unavailable devices, or all devices.																	|
|diagnose			|   Collect diagnostic information and logs.																						|
|erase				|   Erase a device's contents and settings.																							|
|get_app_container	|   Print the path of the installed app's container																					|
|getenv				|   Print an environment variable from a running device.																			|
|help				|   Prints the usage for a given subcommand.																						|
|icloud_sync		|   Trigger iCloud sync on a device.																								|
|install			|   Install an app on a device.																										|
|install_app_data	|   Install an xcappdata package to a device, replacing the current contents of the container.										|
|io					|   Set up a device IO operation.																									|
|keychain			|   Manipulate a device's keychain																									|
|launch				|   Launch an application by identifier on a device.																				|
|list				|   List available devices, device types, runtimes, or device pairs.																|
|location			|   Control a device's simulated location																							|
|logverbose			|   enable or disable verbose logging for a device																					|
|openurl			|   Open a URL in a device.																											|
|pair				|   Create a new watch and phone pair.																								|
|pair_activate		|   Set a given pair as active.																										|
|pbcopy				|   Copy standard input onto the device pasteboard.																					|
|pbpaste			|   Print the contents of the device's pasteboard to standard output.																|
|pbsync				|   Sync the pasteboard content from one pasteboard to another.																		|
|privacy			|   Grant, revoke, or reset privacy and permissions																					|
|push				|   Send a simulated push notification																								|
|rename				|   Rename a device.																												|
|runtime			|   Perform operations on runtimes																									|
|shutdown			|   Shutdown a device.																												|
|spawn				|   Spawn a process by executing a given executable on a device.																	|
|status_bar			|   Set or clear status bar overrides																								|
|terminate			|   Terminate an application by identifier on a device.																				|
|ui					|   Get or Set UI options																											|
|uninstall			|   Uninstall an app from a device.																									|
|unpair				|   Unpair a watch and phone pair.																									|
|upgrade			|   Upgrade a device to a newer runtime.addmedia            Add photos, live photos, videos, or contacts to the library of a device.|
|boot				|   Boot a device or device pair.																									|
|clone				|   Clone an existing device.																										|
|create				|   Create a new device.																											|
|delete				|   Delete specified devices, unavailable devices, or all devices.																	|
|diagnose			|   Collect diagnostic information and logs.																						|
|erase				|   Erase a device's contents and settings.																							|
|get_app_container	|   Print the path of the installed app's container																					|
|getenv				|   Print an environment variable from a running device.																			|
|help				|   Prints the usage for a given subcommand.																						|
|icloud_sync		|   Trigger iCloud sync on a device.																								|
|install			|   Install an app on a device.																										|
|install_app_data	|   Install an xcappdata package to a device, replacing the current contents of the container.										|
|io					|   Set up a device IO operation.																									|
|keychain			|   Manipulate a device's keychain																									|
|launch				|   Launch an application by identifier on a device.																				|
|list				|   List available devices, device types, runtimes, or device pairs.																|
|location			|   Control a device's simulated location																							|
|logverbose			|   enable or disable verbose logging for a device																					|
|openurl			|   Open a URL in a device.																											|
|pair				|   Create a new watch and phone pair.																								|
|pair_activate		|   Set a given pair as active.																										|
|pbcopy				|   Copy standard input onto the device pasteboard.																					|
|pbpaste			|   Print the contents of the device's pasteboard to standard output.																|
|pbsync				|   Sync the pasteboard content from one pasteboard to another.																		|
|privacy			|   Grant, revoke, or reset privacy and permissions																					|
|push				|   Send a simulated push notification																								|
|rename				|   Rename a device.																												|
|runtime			|   Perform operations on runtimes																									|
|shutdown			|   Shutdown a device.																												|
|spawn				|   Spawn a process by executing a given executable on a device.																	|
|status_bar			|   Set or clear status bar overrides																								|
|terminate			|   Terminate an application by identifier on a device.																				|
|ui					|   Get or Set UI options																											|
|uninstall			|   Uninstall an app from a device.																									|
|unpair				|   Unpair a watch and phone pair.																									|
|upgrade			|   Upgrade a device to a newer runtime.																							|
