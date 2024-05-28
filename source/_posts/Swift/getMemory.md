---
title: iOS 获取当前App使用内存
date: 2025-5-28
categories: Swift
icon: swift
---

```swift
import UIKit

func getAppMemoryUsage() -> Double? {
    var info = task_vm_info_data_t()
    var count = mach_msg_type_number_t(MemoryLayout<task_vm_info_data_t>.size) / 4

    let result: kern_return_t = withUnsafeMutablePointer(to: &info) { ptr in
        ptr.withMemoryRebound(to: integer_t.self, capacity: 1) { intPtr in
            task_info(mach_task_self_, task_flavor_t(TASK_VM_INFO), intPtr, &count)
        }
    }
    if result == KERN_SUCCESS {
        let memoryUsageInBytes = info.phys_footprint
        let memoryUsageInMB = Double(memoryUsageInBytes) / (1024 * 1024)
        return memoryUsageInMB
    } else {
        return nil
    }
}
```