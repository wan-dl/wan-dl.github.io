---
title: Tips-001. iOS应用 - 关闭或隐藏键盘
date: 2024-5-23
categories: XCTest
icon: xctest
---

> 以下代码，仅在Swift + SwiftUI开发的应用上测试过。

## 如下图所示，关闭或隐藏键盘

<img src="/images/examples/keyboard.png" style="zoom: 15% !important;" />

## 解决方案

```swift
// 发送回车事件
let el_keyboard = app.keyboards.element
if el_keyboard.exists {
    app.typeText("\n")
}
```