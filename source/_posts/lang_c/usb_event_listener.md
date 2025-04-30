---
title: macosx: 使用c语言监听USB插拔事件
date: 2025-4-30
categories: C语言记录
desc: 
---

## 问题

在MacOSX电脑，使用c语言开发一个USB插拔事件监听程序

## 代码

将下面的代码，保存为usb_event_listener.c文件。

```c
#include <stdio.h>
#include <time.h>
#include <IOKit/IOKitLib.h>
#include <IOKit/usb/IOUSBLib.h>
#include <CoreFoundation/CoreFoundation.h>

// 获取当前时间并格式化为字符串
void getCurrentTime(char *buffer, size_t bufferSize) {
    time_t now = time(NULL);
    struct tm *localTime = localtime(&now);
    strftime(buffer, bufferSize, "%Y-%m-%d %H:%M:%S", localTime);
}

// 插入事件回调函数
void DeviceAddedCallback(void *refCon, io_iterator_t iterator) {
    io_service_t usbDevice;
    while ((usbDevice = IOIteratorNext(iterator))) {
        CFStringRef deviceName = IORegistryEntryCreateCFProperty(
            usbDevice, CFSTR("USB Product Name"), kCFAllocatorDefault, 0);  // 改为更通用的 USB 属性
        if (deviceName) {
            char deviceNameBuffer[256];
            CFStringGetCString(deviceName, deviceNameBuffer, sizeof(deviceNameBuffer), kCFStringEncodingUTF8);

            char timeBuffer[64];
            getCurrentTime(timeBuffer, sizeof(timeBuffer));
            printf("[%s] USB Device Added: %s\n", timeBuffer, deviceNameBuffer);

            CFRelease(deviceName);
        } else {
            char timeBuffer[64];
            getCurrentTime(timeBuffer, sizeof(timeBuffer));
            printf("[%s] USB Device Added: [Unknown Device]\n", timeBuffer);
        }
        IOObjectRelease(usbDevice);
    }
}

// 拔出事件回调函数
void DeviceRemovedCallback(void *refCon, io_iterator_t iterator) {
    io_service_t usbDevice;
    while ((usbDevice = IOIteratorNext(iterator))) {
        char timeBuffer[64];
        getCurrentTime(timeBuffer, sizeof(timeBuffer));
        printf("[%s] USB Device Removed\n", timeBuffer);
        IOObjectRelease(usbDevice);
    }
}

int main() {
    kern_return_t kr;
    mach_port_t masterPort = kIOMainPortDefault; // 使用新的主端口方法

    // 匹配所有 USB 设备
    CFMutableDictionaryRef matchingDict = IOServiceMatching(kIOUSBDeviceClassName);
    if (!matchingDict) {
        fprintf(stderr, "Failed to create matching dictionary.\n");
        return -1;
    }

    // 创建通知端口
    IONotificationPortRef notifyPort = IONotificationPortCreate(masterPort);
    if (!notifyPort) {
        fprintf(stderr, "Failed to create notification port.\n");
        return -1;
    }
    CFRunLoopSourceRef runLoopSource = IONotificationPortGetRunLoopSource(notifyPort);
    CFRunLoopAddSource(CFRunLoopGetCurrent(), runLoopSource, kCFRunLoopDefaultMode);

    // 注册插入通知
    io_iterator_t addedIter;
    kr = IOServiceAddMatchingNotification(
        notifyPort,
        kIOMatchedNotification,
        matchingDict,
        DeviceAddedCallback,
        NULL,
        &addedIter);

    if (kr != KERN_SUCCESS) {
        fprintf(stderr, "Failed to add matching notification.\n");
        return -1;
    }

    // 触发初始插入事件
    DeviceAddedCallback(NULL, addedIter);

    // 注册拔出通知
    CFMutableDictionaryRef removalDict = IOServiceMatching(kIOUSBDeviceClassName);
    io_iterator_t removedIter;
    kr = IOServiceAddMatchingNotification(
        notifyPort,
        kIOTerminatedNotification,
        removalDict,
        DeviceRemovedCallback,
        NULL,
        &removedIter);

    if (kr != KERN_SUCCESS) {
        fprintf(stderr, "Failed to add removal notification.\n");
        return -1;
    }

    // 触发初始拔出事件
    DeviceRemovedCallback(NULL, removedIter);

    // 开启 RunLoop
    printf("Listening for USB events...\n");
    CFRunLoopRun();

    // 清理资源（理论上不会到这里，因为 CFRunLoop 阻塞运行）
    IONotificationPortDestroy(notifyPort);
    return 0;
}
```

## 程序编译

```shell
gcc -o usb_event_listener usb_event_listener.c -framework IOKit -framework CoreFoundation
```

## 程序运行

```shell
./usb_event_listener
```

输出:

```log
[2025-04-30 01:01:01] USB Device Added: Mi USB Receiver
[2025-04-30 01:01:01] USB Device Added: iPhone
[2025-04-30 01:01:01] USB Device Added: USB 10/100 LAN
[2025-04-30 01:01:01] USB Device Added: ikbc Keyboard
[2025-04-30 01:01:01] USB Device Added: USB 2.0 BILLBOARD
[2025-04-30 01:01:01] USB Device Added: Dell MS116 USB Optical Mouse
[2025-04-30 01:01:01] USB Device Added: USB2.1 Hub
[2025-04-30 01:01:01] USB Device Added: USB3.1 Hub
[2025-04-30 01:01:01] USB Device Added: Elements SE SSD
[2025-04-30 01:01:01] USB Device Added: Headset
[2025-04-30 01:01:01] USB Device Added: Apple T2 Controller
[2025-04-30 01:01:01] USB Device Added: AppleUSBXHCI Root Hub Simulation
[2025-04-30 01:01:01] USB Device Added: AppleUSBXHCI Root Hub Simulation
[2025-04-30 01:01:01] USB Device Added: AppleUSBVHCIBCE Root Hub Simulation
Listening for USB events...
[2025-04-30 01:01:01] USB Device Removed
[2025-04-30 01:01:01] USB Device Added: iPhone
[2025-04-30 01:01:01] USB Device Removed
[2025-04-30 01:01:01] USB Device Added: iPhone
```
