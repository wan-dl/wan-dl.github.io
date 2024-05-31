---
title: Tips-006. 侧滑返回上个页面
date: 2025-5-30
categories: XCTest
icon: xctest
---

通过使用 `coordinate(withNormalizedOffset:)` 和 `press(forDuration:thenDragTo:)` 方法，模拟用户的触摸和滑动手势，从而测试应用的侧滑返回功能。

## coordinate(withNormalizedOffset:)

> coordinate文档: [https://developer.apple.com/documentation/xctest/xcuielement/1500960-coordinate](https://developer.apple.com/documentation/xctest/xcuielement/1500960-coordinate)

coordinate(withNormalizedOffset:) 方法用于创建一个相对于屏幕的坐标点。

参数 withNormalizedOffset 是一个 CGVector，其 dx 和 dy 值在 [0, 1] 之间，表示相对于屏幕宽度和高度的归一化坐标。

例如：

- CGVector(dx: 0, dy: 0) 表示屏幕的左上角。
- CGVector(dx: 1, dy: 0) 表示屏幕的右上角。
- CGVector(dx: 0, dy: 1) 表示屏幕的左下角。
- CGVector(dx: 1, dy: 1) 表示屏幕的右下角。

使用这种方式可以方便地定义屏幕上的点，而不需要知道具体的像素值。

## press(forDuration:thenDragTo:)

> press文档：[https://developer.apple.com/documentation/xctest/xcuielement/1618670-press](https://developer.apple.com/documentation/xctest/xcuielement/1618670-press)

press(forDuration:thenDragTo:) 方法用于模拟从一个坐标点开始的长按和拖动手势。

它接受两个参数：

- forDuration: 长按的持续时间（以秒为单位）。
- thenDragTo: 拖动的终点坐标。

## 测试示例

```swift
import XCTest

final class test_SwipeBack: XCTestCase {
    
    var app: XCUIApplication!
    
    override func setUpWithError() throws {
        app = XCUIApplication()
        app.launch()
        continueAfterFailure = false
    }

    override func tearDownWithError() throws {
        app.terminate()
    }
    

    func testExample() throws {
        // 侧滑返回
        let start = app.coordinate(withNormalizedOffset: CGVector(dx: 0, dy: 0.5))
        let end = app.coordinate(withNormalizedOffset: CGVector(dx: 1, dy: 0.5))
        start.press(forDuration: 0.1, thenDragTo: end)
        
        // 断言是否返回到了正确的页面
        XCTAssertTrue(app.navigationBars["Test"].exists)
    }
}
```

## 代码解释

通过 `app.coordinate(withNormalizedOffset:)` 创建了起始和终止坐标，然后使用 `start.press(forDuration:thenDragTo:)` 模拟从起点到终点的拖动手势。

定义起点：

```swift
// 这里的 start 是屏幕左中位置的坐标点（横向 0%，纵向 50%）。
let start = app.coordinate(withNormalizedOffset: CGVector(dx: 0, dy: 0.5))
```

定义终点：

```swift
// 这里的 end 是屏幕右中位置的坐标点（横向 100%，纵向 50%）。
let end = app.coordinate(withNormalizedOffset: CGVector(dx: 1, dy: 0.5))
```

模拟侧滑手势：
```swift
// 这段代码模拟了从 start 点开始长按 0.1 秒，然后拖动到 end 点的手势，即一个从左向右的侧滑操作。
start.press(forDuration: 0.1, thenDragTo: end)
```
