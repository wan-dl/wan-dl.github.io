---
title: python 如何同时执行多个方法?
date: 2025-4-18
categories: Python
icon: Python
desc: 日常使用笔记
---

在 Python 中，可以通过以下方式同时执行多个方法：

### 1. 使用 `threading` 模块
`threading` 模块允许你以线程的方式并发执行多个方法。

```python
import threading

def method1():
    print("Executing method1")

def method2():
    print("Executing method2")

# 创建线程
thread1 = threading.Thread(target=method1)
thread2 = threading.Thread(target=method2)

# 启动线程
thread1.start()
thread2.start()

# 等待线程完成
thread1.join()
thread2.join()
```

### 2. 使用 `concurrent.futures.ThreadPoolExecutor`
`concurrent.futures` 提供了线程池，可以更方便地执行多个任务。

```python
from concurrent.futures import ThreadPoolExecutor

def method1():
    print("Executing method1")

def method2():
    print("Executing method2")

# 使用线程池执行方法
with ThreadPoolExecutor() as executor:
    executor.submit(method1)
    executor.submit(method2)
```

### 3. 使用 `multiprocessing` 模块
如果方法是 CPU 密集型的，可以使用 `multiprocessing` 模块来在多个进程中并行执行。

```python
from multiprocessing import Process

def method1():
    print("Executing method1")

def method2():
    print("Executing method2")

# 创建进程
process1 = Process(target=method1)
process2 = Process(target=method2)

# 启动进程
process1.start()
process2.start()

# 等待进程完成
process1.join()
process2.join()
```

### 4. 使用 `asyncio`（适用于 I/O 密集型任务）
如果方法可以用协程实现，`asyncio` 是一个非常高效的工具。

```python
import asyncio

async def method1():
    print("Executing method1")

async def method2():
    print("Executing method2")

# 并发运行
async def main():
    await asyncio.gather(method1(), method2())

asyncio.run(main())
```

### 选择依据
- **I/O 密集型任务**（如网络请求、文件读写）：推荐使用 `asyncio` 或 `threading`。
- **CPU 密集型任务**（如数据处理、计算）：推荐使用 `multiprocessing`。