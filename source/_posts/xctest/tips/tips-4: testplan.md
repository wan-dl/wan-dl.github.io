---
title: Tips-004. XCTest - testPlan 使用测试计划
date: 2025-4-27
categories: XCTest
icon: xctest
---

成熟的项目受益于涵盖各种场景的众多测试。测试可以验证代码行为或衡量App性能。

通过创建配置测试计划，您可以控制在软件工程流程的不同阶段从测试中获得的信息。

> 例如，您可以创建01测试计划，在开发和调试模块时仅运行该模块的单元测试。创建02测试计划，在将 app 提交到 App Store 之前运行所有单元测试、集成测试和 UI 测试。

<img src="/images/xcode/organizing-tests-hero.png" style="border: none !important;" />

## 了解测试计划

testPlan 是Xcode 项目中的一份文档，其中描述了当开发者调用测试操作时 Xcode 应该运行哪些测试（如下图所示），以及 Xcode 用于运行测试的配置。

<img src="/images/xcode/organizing-tests-test-plan.png" style="border: none !important;" />

您可以为同一个方案创建多个测试计划，并在从 Xcode 或终端运行测试时使用其中任何一个。您必须选择一个测试计划作为该方案的默认计划；当未明确指定任何计划时，Xcode 将使用此计划运行测试。

测试计划是一个带有`.xctestplan`扩展名的JSON文件。

**测试计划可以做什么？或有什么强大的地方？**

- 控制测试范围。比如选择一组测试用例。
- 测试多个本地化：为您支持的每种语言创建配置。

## 如何创建测试计划？
---------------------

Xcode顶部菜单 [Product] -> [Test Plan] -> [New Test Plan ...]

<img src="/images/xcode/create_testplan_1.jpg" style="zoom: 40% !important;" />


## 编辑测试计划

选中testPlan打开，就可以编辑。

如下图，编辑测试计划：

- Tests: 可以选择要运行的测试用例文件
- Configurations: 可以配置启动参数、本地化设置、屏幕截图设置、文本执行（字母或随机）、运行时调试选项Sanitizer、线程检查器和malloc守卫。

<img src="/images/xcode/testplan_edit.jpg" style="zoom: 40% !important;" />

## 管理测试计划

Xcode顶部菜单 [Product] -> [Test Plan] -> [Manage Test Plan …]

<img src="/images/xcode/test_plan_manage.jpg" style="zoom: 40% !important;" />

## 查看.xctestplan文件

<img src="/images/xcode/testplan_file.jpg" style="zoom: 40% !important;" />

## 扩展文档

- [测试计划configurations配置项](https://developer.apple.com/documentation/xcode/organizing-tests-to-improve-feedback)
- [使用命令行运行xctest测试](https://developer.apple.com/documentation/xcode/organizing-tests-to-improve-feedback#Run-tests-from-the-command-line)