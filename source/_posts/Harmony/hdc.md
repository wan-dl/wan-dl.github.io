---
title: Harmony hdc命令行工具
date: 2025-3-18
categories: Harmony
icon: Harmony
---

## 获取连接的设备信息

```shell
hdc list targets

# 获取设备uuid
hdc shell bm get --udid
```

## 获取指定进程内存

```shell
hdc -t <deviceID> shell SP_daemon -N 1 -PKG io.dcloud.uniappx -r
```

## 手机常亮、唤醒设备(点亮手机屏幕)

```shell

// 设备保持常亮
hdc -t <deviceID> shell power-shell setmode 602

// 唤醒设备(点亮手机屏幕)
hdc -t <deviceID> shell power-shell wakeup

// 查看屏幕状态
hdc shell hidumper -s 3301 -a -a
```


## 获取设备已安装的应用列表

```shell
hdc -t <deviceID> shell bm dump -a
```

## 关闭整个应用

```shell
hdc -t <deviceID>  shell aa force-stop <package-name>

// 示例
hdc shell aa force-stop io.dcloud.uniappx
```

## 清除应用数据、缓存

```shell

// 清除应用数据
hdc shell bm clean -d -n <package-name>

// 清除应用缓存
hdc shell bm clean -c -n <package-name>
```

## 截屏

```shell
// -f表示指定图片在设备上的存储路径，如不指定，会在命令执行完成后显示图片默认存储路径
hdc shell snapshot_display -f /data/local/tmp/test.jpeg 

// 发送手机图片到电脑
hdc file recv /data/local/tmp/test.jpeg $HOME/
```

## 获取手机应用页面布局信息（控件树）

```shell
hdc shell uitest dumpLayout
```

输出的json文件信息：

```
{
    "attributes": {
        "accessibilityId": "",
        "backgroundColor": "",
        "backgroundImage": "",
        "blur": "",
        "bounds": "[0,0][1260,2720]",
        "checkable": "",
        "checked": "",
        "clickable": "",
        "description": "",
        "enabled": "",
        "focused": "",
        "hitTestBehavior": "",
        "hostWindowId": "",
        "id": "",
        "key": "",
        "longClickable": "",
        "opacity": "",
        "origBounds": "",
        "scrollable": "",
        "selected": "",
        "text": "",
        "type": "",
        "zIndex": ""
    },
    "children": {}
}
```

## HDC注入UI模拟操作

相关文档 [https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkxtest-guidelines](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/arkxtest-guidelines)

```shell
// 查看帮助文档
hdc shell uitest uniInput -h
```

- 屏幕点击
```shell
hdc shell uitest uiInput click 657 1556
```

- keyEvent

```shell
// 返回主页
hdc shell uitest uiInput keyEvent Home
```

keyEvent映射表参考：[https://docs.openharmony.cn/pages/v4.1/en/application-dev/reference/apis-input-kit/js-apis-keycode.md](https://docs.openharmony.cn/pages/v4.1/en/application-dev/reference/apis-input-kit/js-apis-keycode.md)