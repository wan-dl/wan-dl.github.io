---
title: Tips-005. 测试slider
date: 2024-5-30
categories: XCTest
icon: xctest
---

## 页面UI

<img src="/images/examples/slider.png" style="zoom: 30% !important;" />

## 页面源码

```swift
import SwiftUI

struct t_silder: View {
    
    @State private var speed: Float = 50.0
    @State private var isEditing = false
    
    var body: some View {
        VStack {
            Slider(
                value: $speed,
                in: 0...100,
                onEditingChanged: { editing in
                    isEditing = editing
                }
            )
            .accessibilityIdentifier("SpeedSlider")
            
            Text("\(speed)")
                .foregroundColor(isEditing ? .red : .blue)
                .accessibilityIdentifier("SpeedText")
        }
    }
}
```

## 测试代码

```
import XCTest

final class test_slider: XCTestCase {
    
    var app: XCUIApplication!
    
    override func setUpWithError() throws {
        app = XCUIApplication()
        app.launch()
    }

    override func tearDownWithError() throws {
    }

    func testExample() throws {        
        let slider = app.sliders["SpeedSlider"]
        let text = app.staticTexts["SpeedText"]
       
        // Initial value test
        XCTAssertTrue(text.label.contains("50"), "初始值包含50")

        // Interact with the slider
        slider.adjust(toNormalizedSliderPosition: 0.75) // Set slider to 75%

        // Check the new value and color while editing
        XCTAssertTrue(text.label.contains("75"), "调整值包含75")
        
    }
}
```
