---
title: uni-app x: 使用uni.getPerformance()获取app启动时间和排版渲染时间
date: 2025-4-24
categories: uni-app-x
icon: uni-app-x
desc: 记录uni-app自动化测试系列
---

### 介绍

关于uni.getPerformance()的介绍: [https://doc.dcloud.net.cn/uni-app-x/api/get-performance.html](https://doc.dcloud.net.cn/uni-app-x/api/get-performance.html#uni-getperformance)

### 如何使用？

打开uni-app x项目，在app.uvue文件onLaunch节点，插入如下代码。

```js

<script lang="uts">
    export default {
        onLaunch: function () {
            console.log('App Launch');
            const performance = uni.getPerformance()
            const observer1 : PerformanceObserver = performance.createObserver((entryList : PerformanceObserverEntryList) => {
                console.log('data: ', entryList.getEntries());
            });
            observer1.observe({ entryTypes: ['render', 'navigation'] } as PerformanceObserverOptions);

        },
        onShow: function () {
            console.log('App Show')
        },
        onHide: function () {
            console.log('App Hide')
        },
        // #ifdef APP-ANDROID
        onLastPageBackPress: function () {
            console.log('App LastPageBackPress');
        },
        // #endif
        onExit: function () {
            console.log('App Exit')
        },
    }
</script>
```