---
title: logging
date: 2023-5-27
categories: Python
icon: Python
---

# logging

logging：这个模块为应用与库实现了灵活的事件日志系统的函数与类。

简单示例:

```python
$ import logging
$ logging.warning('Watch out!')
$ WARNING:root:Watch out!
```

## 日志级别

默认分为六种日志级别（括号为级别对应的数值）：

- CRITICAL	50		
- ERROR		40		
- WARNING	30		
- INFO		20		
- DEBUG		10		
- NOTSET	0		


注意：自定义日志级别时注意不要和默认的日志级别数值相同，logging 执行时输出大于等于设置的日志级别的日志信息，如设置日志级别是 INFO，则 INFO、WARNING、ERROR、CRITICAL 级别的日志都会输出。

## 常用示例

```python
import sys
import logging

# 封装为一个方法以便在其他文件中调用
def setup_logging(log_file=".log"):
    logging.basicConfig(
        level=logging.DEBUG,
        format="%(asctime)s %(levelname)s: %(message)s",
        datefmt="%Y-%H-%M %H:%M:%S",
        handlers=[
            logging.FileHandler(log_file),
            logging.StreamHandler(sys.stdout)
        ]
    )
    return logging.getLogger(__name__)

logger = setup_logging('test.log')

logger.debug('debug message')
logger.info('info message')
logger.warning('warning message')
logger.error('error message')
logger.critical('critical message')
```

如上代码，需要注意的地方：

1. logging.basicConfig(**kwargs): 用于配置 logging 模块的基本行为。`**kwargs` 是一个可变的关键字参数，可以用来传递各种配置选项。
2. 代码中用到了logggin.handlers, 做了两个事情：将日志记录输出到控制台或终端、并输出到磁盘文件。

### logging.basicConfig

logging.basicConfig(**kwargs) 是 Python logging 模块中的一个函数，用于配置 logging 模块的基本行为。**kwargs 是一个可变的关键字参数，可以用来传递各种配置选项。

baseConfig 是 basicConfig 函数中的一个参数，它是一个字典，用于指定 logging 模块的基本配置。该字典包括以下键值对：

- filename：指定日志输出文件的文件名。
- filemode：指定日志输出文件的打开模式，默认为 a（追加模式）。
- format：指定日志输出格式的字符串。
- datefmt：指定日期/时间格式的字符串。
- level：指定日志输出的最低级别。低于该级别的日志消息将被忽略。
- stream：指定日志输出流。默认为 sys.stderr。
- handlers
- encoding

使用 basicConfig 函数可以很方便地配置 logging 模块的基本行为，例如设置日志输出的格式、级别、输出流等。

但是需要注意，该函数只能在程序中调用一次，多次调用会导致 logging 模块的行为不可预测。

### logging.handlers

logging.handlers 日志处理程序。[官方文档教程](https://docs.python.org/zh-cn/3/library/logging.handlers.html#module-logging.handlers)

- StreamHandler: 它可将日志记录输出发送到数据流例如 sys.stdout, sys.stderr 或任何文件类对象
- FileHandler: 它可将日志记录输出到磁盘文件中。 它从 StreamHandler 继承了输出功能。

## 改进

如上logging代码，输出到终端或控制台的日志是不带有颜色的。如果想带有颜色，怎么处理？

要使输出到终端的日志带有颜色，可以使用 `colorlog` 模块。首先需要安装 `colorlog` 模块，可以使用以下命令进行安装：

```
pip install colorlog
```

然后，可以使用以下代码来改进你的代码：

```python
import sys
import logging
import colorlog

def setup_logging(log_file=".log"):
    handler = colorlog.StreamHandler(sys.stdout)
    handler.setFormatter(colorlog.ColoredFormatter(
        "%(log_color)s%(asctime)s %(levelname)s: %(message)s",
        datefmt="%Y-%H-%M %H:%M:%S",
        log_colors={
            'DEBUG': 'cyan',
            'INFO': 'green',
            'WARNING': 'yellow',
            'ERROR': 'red',
            'CRITICAL': 'bold_red',
        },
        secondary_log_colors={},
        style='%'
    ))
    logger = logging.getLogger(__name__)
    logger.addHandler(handler)
    logger.setLevel(logging.DEBUG)
    return logger

logger = setup_logging('test.log')

logger.debug('debug message')
logger.info('info message')
logger.warning('warning message')
logger.error('error message')
logger.critical('critical message')
```

这个代码使用了 `colorlog` 模块来创建一个 `StreamHandler`，然后使用 `ColoredFormatter` 格式化日志输出。

`log_colors` 参数指定了不同级别的日志输出使用的颜色，`secondary_log_colors` 参数可以用来指定更多的颜色。

最后，将 `StreamHandler` 添加到 logger 中，并设置 logger 的日志级别为 `logging.DEBUG`。这样就可以输出带有颜色的日志信息了。

