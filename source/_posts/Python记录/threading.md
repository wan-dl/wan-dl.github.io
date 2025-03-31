---
title: threading
date: 2025-03-31
categories: Python
icon: Python
desc: 日常使用笔记
---

```python
import threading
import time

# 创建一个事件对象，用于线程停止控制
stop_event = threading.Event()

def test_sleep():
    """
    模拟一个耗时操作，比如某个函数调用，需要等待3秒
    """
    print("test_sleep started")
    time.sleep(3)
    print("test_sleep finished")

def keep_wakeup(device_id, stop_event):
    """
    模拟设备定时唤醒的线程函数。每隔50秒打印一次唤醒信息，直到stop_event被设置。
    
    参数:
    device_id (str): 设备ID
    stop_event (threading.Event): 控制线程停止的事件
    """
    while not stop_event.is_set():
        # 模拟工作
        if not stop_event.is_set():
            print(f"Device {device_id} is waking up")

        # 这里的stop_event.wait(50)将会等待50秒或直到stop_event被设置
        stop_event.wait(50)

# 模拟的设备ID
device_id = "9DCDKJOIJLKCHLJ"

# 创建并启动设备唤醒线程
t = threading.Thread(target=keep_wakeup, args=(device_id, stop_event))
t.start()

# 执行test_sleep，模拟主线程的耗时操作
test_sleep()

# 在test_sleep方法结束后，设置stop_event来通知线程停止
stop_event.set()

# 等待设备唤醒线程结束
t.join()

print("Thread has been stopped")
```

### 代码注释解释

1. **导入模块**:
   - `threading`: 用于创建和操作线程。
   - `time`: 用于时间相关操作，比如休眠。

2. **创建事件对象**:
   - `stop_event = threading.Event()`: 用于控制设备唤醒线程的停止。

3. **定义`test_sleep`函数**:
   - 模拟一个耗时操作，比如某个函数调用，需要等待3秒。

4. **定义`keep_wakeup`函数**:
   - 这是一个线程函数，模拟设备每隔50秒打印一次唤醒信息，直到`stop_event`被设置。
   - 使用`stop_event.wait(50)`来实现等待50秒或直到事件被设置。

5. **设备ID**:
   - `device_id = "9DCDKJOIJLKCHLJ"`: 模拟的设备ID。

6. **创建并启动设备唤醒线程**:
   - 使用`threading.Thread`创建线程，并使用`start`方法启动线程。

7. **执行`test_sleep`函数**:
   - 模拟主线程的耗时操作。

8. **停止设备唤醒线程**:
   - `stop_event.set()`设置事件，通知设备唤醒线程停止。
   - 使用`t.join()`等待设备唤醒线程结束。

9. **打印线程停止信息**:
   - `print("Thread has been stopped")`表示设备唤醒线程已经结束。

通过这个示例，可以学习如何创建和控制线程，如何使用事件对象来控制线程的停止，并理解线程间的同步机制。