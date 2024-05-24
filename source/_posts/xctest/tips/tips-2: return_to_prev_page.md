---
title: Tips-002. iOS应用 - 返回到上个页面
date: 2025-5-23
categories: XCTest
icon: xctest
---

> 以下代码，仅在Swift + SwiftUI开发的应用上测试过。

## 如下图所示，点击Back，返回上个页面

<img src="/images/examples/return_previous_page.png" style="zoom: 15% !important;" />

## 解决方案

```swift
import XCTest

final class testUITestsInput: XCTestCase {
    
    var app: XCUIApplication!
    
    override class var runsForEachTargetApplicationUIConfiguration: Bool {
        true
    }
    
    override func setUpWithError() throws {
        app = XCUIApplication()
        app.launch()
    }

    func testBack() throws {
        // 进入某个页面
        let el_input = app.buttons["input"]
        XCTAssertTrue(el_input.exists, "Element: Input does not exist")
        guard el_input.exists else {
            return
        }
        el_input.tap()
        
        // 返回到上个页面
        app.navigationBars.buttons.element(boundBy: 0).tap()        
    }
}
```