---
title: iOS xctest metrics测试指标
date: 2025-5-22
categories: 使用 XCTest 测试 iOS App
---

### metrics测试指标

文档: [https://developer.apple.com/documentation/xctest/xctmetric](https://developer.apple.com/documentation/xctest/xctmetric)

|class						|描述											|
|--							|--												|
|XCTCPUMetric				|记录性能测试期间 CPU 活动信息的指标					|
|XCTClockMetric				|记录性能测试期间所用时间的指标。						|
|XCTMemoryMetric			|记录性能测试使用的物理内存的指标。					|
|XCTOSSignpostMetric		|用于记录性能测试执行带路标的代码区域所花费的时间的指标。	|
|XCTStorageMetric			|记录性能测试逻辑写入存储的数据量的指标。				|
|XCTApplicationLaunchMetric	|记录性能测试的应用程序启动持续时间的指标				|


代码示例:

``` swift
import XCTest

final class PerformanceTests: XCTestCase {

    override func setUpWithError() throws {
        continueAfterFailure = false
    }

    override func tearDownWithError() throws {
        // Put teardown code here. This method is called after the invocation of each test method in the class.
    }

    func testLaunchPerformance() throws {
        if #available(macOS 10.15, iOS 13.0, tvOS 13.0, watchOS 7.0, *) {
            // This measures how long it takes to launch your application.
            measure(metrics: [XCTApplicationLaunchMetric()]) {
                XCUIApplication(bundleIdentifier: "io.dcloud.uniappx").launch()
            }
        }
    }
}
```