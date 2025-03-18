---
title: SwiftUI @ObservedObject 和@StateObject的区别
date: 2024-506-14
categories: Apple Swift
icon: swift
---

> 备注以下内容，始于ChatGPT 4o 。觉得非常有用，因此收录。

在 SwiftUI 中，@ObservedObject 和 @StateObject 都用于声明和管理观察对象，但是它们有不同的用途和生命周期管理方式。下面是它们的区别和使用场景：

## @ObservedObject

使用场景：当一个视图依赖于外部对象的数据变化，并且该对象由视图的父视图创建和管理时，使用 @ObservedObject。

生命周期：@ObservedObject 的生命周期由外部管理，它不会负责对象的创建和销毁。当 @ObservedObject 的对象发生变化时，视图会自动更新。

示例：

```swift
class MyViewModel: ObservableObject {
    @Published var data: String = "Initial Data"
}

struct ParentView: View {
    @StateObject private var viewModel = MyViewModel()
    
    var body: some View {
        ChildView(viewModel: viewModel)
    }
}

struct ChildView: View {
    @ObservedObject var viewModel: MyViewModel
    
    var body: some View {
        Text(viewModel.data)
    }
}
```

在这个示例中，ParentView 创建并管理 MyViewModel 对象，而 ChildView 通过 @ObservedObject 来观察 viewModel 的变化。

## @StateObject

使用场景：当一个视图需要自己创建和管理一个状态对象，并且确保对象在视图的生命周期内存在时，使用 @StateObject。

生命周期：@StateObject 负责对象的创建和销毁。它确保对象在视图生命周期内只被创建一次，并在视图销毁时被销毁。

示例：

```swift
class MyViewModel: ObservableObject {
    @Published var data: String = "Initial Data"
}

struct ContentView: View {
    @StateObject private var viewModel = MyViewModel()
    
    var body: some View {
        Text(viewModel.data)
    }
}
```

在这个示例中，ContentView 自己创建并管理 MyViewModel 对象，使用 @StateObject 来确保在视图的整个生命周期内 viewModel 只被创建一次。

## 什么时候使用哪个？

使用 @ObservedObject：当你的对象由父视图创建和管理，并传递给子视图时，使用 @ObservedObject。这意味着子视图只是观察该对象的变化，而不负责其生命周期管理。

使用 @StateObject：当你的对象需要在视图内部创建，并且需要由该视图管理其生命周期时，使用 @StateObject。这样可以确保对象在视图的整个生命周期内是稳定的，并且不会因为视图的重新创建而多次创建对象。

总结起来，@StateObject 用于管理和持有状态对象，而 @ObservedObject 用于观察由其他视图持有和管理的状态对象。理解它们的区别和正确使用场景有助于有效地管理视图和数据的生命周期，从而避免内存泄漏和不必要的资源消耗。