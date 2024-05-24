---
title: iOS启动时间测试调研记录
date: 2025-5-21
categories: iOS
icon: apple
---

## 1. 使用xctest measure

xctest 支持编写性能测试来收集一段代码执行所用的时间、执行期间所用的内存或所写的数据这些方面的信息。
XCTest 会多次运行你的代码来测量请求的指标。
你可以为指标设置一个基准预期，如果测得的值比基准值差太多，XCTest 会报告测试失败。

具体代码:

```swift
import XCTest

final class PerformanceTests: XCTestCase {

    override func setUpWithError() throws {
        continueAfterFailure = false
    }

    override func tearDownWithError() throws {
        XCUIApplication(bundleIdentifier: "io.dcloud.uniappx").terminate()
    }

    func testLaunchPerformance() throws {
        if #available(macOS 10.15, iOS 13.0, tvOS 13.0, watchOS 7.0, *) {
            measure(metrics: [XCTApplicationLaunchMetric(), XCTMemoryMetric()]) {
                XCUIApplication(bundleIdentifier: "io.dcloud.uniappx").launch()
            }
        }
    }
}
```


请注意：下图，标红部分，这里传入了bundleID，用于启动特定的应用。


![](/images/xcode-measure-code.jpg)

Xcode控制台输出的测试日志：

![](/images/xcode-measure-result.jpg)

当然，选择测试用例，右键菜单【Jump to Report】也可以看到测试报告。

![](/images/xcode-xctest-report-metrics.jpg)


## 2. 使用Xcode Instruments工具

Instrument是 苹果官方IDE Xcode 自带的 调试工具。
访问入口:Xcode 顶部菜单 【Xcode】->【Open Developer tool】->【lnstruments】。

![](/images/xcode-menus-toos.jpg)

如下图所示，我们研究启动时间，主要用到两个。

- App Launch：用于查看App的启动过程，从而可以针对性的对启动速度进行优化
- Time Profiler：时间分析器，对运行在系统cpu上的进程执行基于低开销时间的采样

![](/images/xcode-instruments.jpg)

![](/images/instruments-time-profiler.jpg)

## 3. Xcode 配置DYLD环境变量

>注意：DYLD_PRINT_STATISTICS 仅适用于iOS 15以下。iOS以上不起作用。

DYLD 是动态链接器，可在执行应用程序时加载和链接应用程序的共享库。

- DYLD_PRINT_STATISTICS：录有关应用程序启动过程的统计信息，例如，应用程序启动完成后加载了多少镜像
- DYLD_PRINT_STATISTICS_DETAILS：记录动态加载程序何时调用构造函数程序和析构函数

![](/images/xcode-edit-schema.jpg)

实际效果:

![](/images/xcode-console-time.jpg)
