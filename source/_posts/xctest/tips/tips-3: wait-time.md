---
title: Tips-003. iOS应用 - 等待时间或等待元素出现
date: 2025-5-23
categories: XCTest
icon: xctest
---

> 以下代码，仅在Swift + SwiftUI开发的应用上测试过。

## 只等待，什么也不做
------------

```swift
Thread.sleep(forTimeInterval: TimeInterval)

Thread.sleep(forTimeInterval: 30)
```


## 等待元素出现
----------------

在 XCTest 中等待元素， 通常是指在 UI 测试中等待应用程序中的特定 UI 元素出现或符合特定条件。

这在自动化 UI 测试中非常重要，因为应用程序的响应时间可能会有所不同，元素的出现也可能有延迟。


### 使用waitForExistence

waitForExistence 在超时时间内等待元素出现。如果超时时间已到而元素尚未存在，则返回False。

```swift
let element = app.buttons["Login Btn"]
XCTAssertTrue(element.waitForExistence(timeout: 10))
```