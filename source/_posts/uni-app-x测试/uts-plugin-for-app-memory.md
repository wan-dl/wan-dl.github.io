---
title: uni-app x 编写uts插件，获取app内存
date: 2025-3-19
categories: uni-app-x
icon: uni-app-x
desc: 记录uni-app自动化测试系列
---

Git仓库地址：[http://github.com/wan-dl/uniapp-x-performance.git](http://github.com/wan-dl/uniapp-x-performance.git)

## 创建uts插件

uts插件名：app-performance
interface文件：app-performance/utssdk/interface.uts

```ts
export type getMemory = () => any
```

插件目录结构:

```
app-performance
├── changelog.md
├── package.json
├── readme.md
└── utssdk
    ├── app-android
    │   ├── config.json
    │   └── index.uts
    ├── app-harmony
    │   └── index.uts
    ├── app-ios
    │   ├── config.json
    │   ├── getAppMemoryUsage.swift
    │   └── index.uts
    └── interface.uts

5 directories, 10 files
```

## 获取Harmony应用内存

相关文档: [https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V13/js-apis-hidebug-V13#hidebuggetpss](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V13/js-apis-hidebug-V13#hidebuggetpss)

创建app-harmony/index.uts文件，输入如下内容：

```ts
import { getMemory } from '../interface.uts';
import { hidebug } from '@kit.PerformanceAnalysisKit';

export const getAppMemory : getMemory = function () : UTSJSONObject {
    let result  = new UTSJSONObject({
        "TotalPss": 0
    });
    
    let pss: bigint = hidebug.getPss();
    let pssv: string = (Number(pss) / 1024).toFixed(2);
    result['TotalPss'] = pssv;
    return result;
}
```

## 获取Android应用内存

创建app-android/index.uts文件，输入如下内容：

```ts
import { getMemory } from '../interface.uts'

import Process from 'android.os.Process';
import Context from 'android.content.Context';
import ActivityManager from "android.app.ActivityManager"

export const getAppMemory : getMemory = function () : UTSJSONObject {

    let result : UTSJSONObject = {
        "TotalPss": 0,
        "TotalPrivateDirty": 0,
        "TotalSharedDirty": 0
    }

    // 获取系统的 ActivityManager 服务
    // const activityManager= requireContext().getSystemService(Context.ACTIVITY_SERVICE) as ActivityManager;
    const activityManager = UTSAndroid.getAppContext()!.getSystemService(Context.ACTIVITY_SERVICE) as ActivityManager;

    // 获取当前进程的 PID（进程 ID）
    // const pidArray = new IntArray([Process.myPid()]);
    const pidArray = [Process.myPid()].toKotlinList().toIntArray();
    const memoryInfoArray = activityManager.getProcessMemoryInfo(pidArray);

    // 获取当前进程的内存信息
    const memoryInfo = memoryInfoArray[0];

    // 获取总的 PSS（Proportional Set Size），并将其从 KB 转换为 MB
    const totalPss = (memoryInfo.getTotalPss().toFloat() / 1024).toFixed(2);
    result["TotalPss"] = totalPss;

    // 获取总的私有脏页内存，并将其从 KB 转换为 MB
    const totalPrivateDirty = (memoryInfo.getTotalPrivateDirty().toFloat() / 1024).toFixed(2);
    result["TotalPrivateDirty"] = totalPrivateDirty;

    // 获取总的共享脏页内存，并将其从 KB 转换为 MB
    const totalSharedDirty = (memoryInfo.getTotalSharedDirty().toFloat() / 1024).toFixed(2);
    result["TotalSharedDirty"] = totalSharedDirty;

    // console.log(result)
    // 返回包含内存使用信息的字符串
    // return `Total PSS: ${totalPss}MB\nTotal Private Dirty: ${totalPrivateDirty}MB\nTotal Shared Dirty: ${totalSharedDirty}MB`;
    return result;
}
```

## 获取iOS应用内存

创建一个app-ios/getAppMemoryUsage.swift文件，输入如下内容

```swift
import UIKit

func getAppMemoryUsage() -> Double? {
    var info = task_vm_info_data_t()
    var count = mach_msg_type_number_t(MemoryLayout<task_vm_info_data_t>.size) / 4

    let result: kern_return_t = withUnsafeMutablePointer(to: &info) {
        $0.withMemoryRebound(to: integer_t.self, capacity: 1) {
            task_info(mach_task_self_, task_flavor_t(TASK_VM_INFO), $0, &count)
        }
    }

    if result == KERN_SUCCESS {
        let memoryUsageInBytes = info.phys_footprint
        let memoryUsageInMB = Double(memoryUsageInBytes) / (1024 * 1024)
        return Double(round(100 * memoryUsageInMB) / 100)
    } else {
        return nil
    }
}
```

创建index.uts文件，输入如下内容：

```ts
import { getMemory } from '../interface.uts'

export const getAppMemory : getMemory = function () : UTSJSONObject {
    
    let result : UTSJSONObject = {
        "TotalPss": 0
    };
    
    let val = getAppMemoryUsage()
    if (val != null) {
        result["TotalPss"] = val;
    };
    return result; 
}
```

## 如何在uvue文件调用？

```vue
<template>
    <view>
        <button @click='getData()'>获取app内存</button>
        
        <view>App使用内存：{{ memoryData.TotalPss }} MB</view>
    </view>
</template>

<script setup lang="uts">
    import { getAppMemory } from '@/uni_modules/app-performance';
    
    type Obj = {
        TotalPss: string;
        TotalPrivateDirty: string;
        TotalSharedDirty: string;
    };
    let memoryData = ref<Obj>({
        TotalPss: '0',
        TotalPrivateDirty: '',
        TotalSharedDirty: ''
    });

    function getData() {
        let data: any = getAppMemory();
        console.log("-->", data );
        memoryData.value.TotalPss = (data as UTSJSONObject)["TotalPss"] as String;
    };
</script>

<style>
    .button {
        margin: 15px;
    }
</style>
```