---
title: Kotlin 获取Android应用内存
date: 2025-5-28
categories: Android
icon: android
---


```kotlin
private fun getMemoryInfo(): String {
    // 获取系统的 ActivityManager 服务
    val activityManager = requireContext().getSystemService(Context.ACTIVITY_SERVICE) as ActivityManager

    // 获取当前进程的 PID（进程 ID）
    val memoryInfoArray = activityManager.getProcessMemoryInfo(intArrayOf(android.os.Process.myPid()))

    // 获取当前进程的内存信息
    val memoryInfo = memoryInfoArray[0]

    // 获取总的 PSS（Proportional Set Size），并将其从 KB 转换为 MB
    val totalPss = memoryInfo.totalPss / 1024

    // 获取总的私有脏页内存，并将其从 KB 转换为 MB
    val totalPrivateDirty = memoryInfo.totalPrivateDirty / 1024

    // 获取总的共享脏页内存，并将其从 KB 转换为 MB
    val totalSharedDirty = memoryInfo.totalSharedDirty / 1024

    // 返回包含内存使用信息的字符串
    return "Total PSS: ${totalPss}MB\nTotal Private Dirty: ${totalPrivateDirty}MB\nTotal Shared Dirty: ${totalSharedDirty}MB"
}
```