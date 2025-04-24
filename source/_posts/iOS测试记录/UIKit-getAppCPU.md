---
title: Swift+UIKit获取iOS应用CPU
date: 2025-4-24
categories: iOS
icon: apple
---

```swift
import UIKit

func getCPUUsage() -> Double {
    var kr: kern_return_t
    var task_info_count: mach_msg_type_number_t

    task_info_count = mach_msg_type_number_t(TASK_INFO_MAX)
    var tinfo = task_basic_info()
    var task_info_out = [integer_t](repeating: 0, count: Int(task_info_count))

    kr = withUnsafeMutablePointer(to: &tinfo) {_ in 
        task_info_out.withUnsafeMutableBufferPointer {
            task_info(mach_task_self_, task_flavor_t(TASK_BASIC_INFO), $0.baseAddress, &task_info_count)
        }
    }

    if kr != KERN_SUCCESS {
        return -1
    }

    var thread_list: thread_act_array_t?
    var thread_count = mach_msg_type_number_t()

    kr = withUnsafeMutablePointer(to: &thread_list) { thread_listPtr in
        thread_listPtr.withMemoryRebound(to: (thread_act_array_t?).self, capacity: 1) { reboundedPtr in
            task_threads(mach_task_self_, reboundedPtr, &thread_count)
        }
    }

    if kr != KERN_SUCCESS {
        return -1
    }

    var tot_cpu: Double = 0

    if let thread_list = thread_list {
        for i in 0..<thread_count {
            var thinfo = thread_basic_info()
            var thread_info_count = mach_msg_type_number_t(THREAD_INFO_MAX)
            kr = withUnsafeMutablePointer(to: &thinfo) {
                $0.withMemoryRebound(to: integer_t.self, capacity: 1) { reboundedPtr in
                    thread_info(thread_list[Int(i)], thread_flavor_t(THREAD_BASIC_INFO), reboundedPtr, &thread_info_count)
                }
            }

            if kr != KERN_SUCCESS {
                return -1
            }

            if (thinfo.flags & TH_FLAGS_IDLE) == 0 {
                tot_cpu += (Double(thinfo.cpu_usage) / Double(TH_USAGE_SCALE)) * 100.0
            }
        }
    }

    return tot_cpu
}
```