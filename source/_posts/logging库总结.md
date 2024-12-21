---
title: logging库总结
date: '2024-12-21 19:36:20'
math: false
excerpt: ''
tags:
- python
- input
- permanent
- linux
---

# logging库

## log level

logging库定义了以下常量，作为log level

```
CRITICAL = 50
FATAL = CRITICAL
ERROR = 40
WARNING = 30
WARN = WARNING
INFO = 20
DEBUG = 10
NOTSET = 0
```

## 基本的类

- Logger 记录器
- LogRecoder
- Handler 处理器
- Filter 过滤器
- Formatter 格式器

中文名来自[python文档](https://docs.python.org/zh-cn/3/howto/logging.html)

### Logger

在每个需要记录的module里用`__name__`初始化logger，这意味着记录器名称跟踪包或模块的层次结构，并且直观地从记录器名称显示记录事件的位置。

```
logger = logging.getLogger(__name__)
```

logger有自己的name, level, handlers，也有父logger，要向上级传递信息。有以下方法

- setLevel() 设置logger的level。
- debug(), info()等 输出对应level的信息
- log(level, msg)
- exception() 记录当前的堆栈追踪，仅从异常处理程序调用此方法。
- findCaller() 
  通过currentframe()方法找到stack frame of the caller so that we can note the source file name, line number and function name.
- makeRecord()
  A factory method which can be overridden in subclasses to create specialized LogRecords.
- handle()
  如果record可以通过所有filter就callHandlers，把record发给所有handlers
- addHandler(), removeHandler() 增减指定handler

> 如果未在记录器上显式设置级别，则使用其父记录器的级别作为其有效级别。如果父记录器没有明确的级别设置，则检查 *其* 父级。依此类推，搜索所有上级元素，直到找到明确设置的级别。

> 子记录器将消息传播到与其父级记录器关联的Handlers。因此，不必为应用程序使用的所有记录器定义和配置Handlers。一般为顶级记录器配置Handlers，再根据需要创建子记录器就足够了。（但是，你可以通过将记录器的 *propagate* 属性设置为 `False` 来关闭传播。）

### LogRecord

一个LogRecord示例表示一个被记录的event

### Handler

- setLevel() logger中设置的level确定将传递给其Handler的**消息的最低严重性**。每个Handler中设置的级别确定该Handler将发送哪些级别的消息。
- setFormatter()设置该Handler使用的 Formatter 
- addFilter()/removeFilter()
### Formatter

https://docs.python.org/zh-cn/3/howto/logging.html#formatters

### Filter

`Filters` 可被 `Handlers` 和 `Loggers` 用来实现比按层级提供更复杂的过滤操作。 基本过滤器类只允许低于日志记录器层级结构中低于特定层级的事件。 例如，一个用 'A.B' 初始化的过滤器将允许 'A.B', 'A.B.C', 'A.B.C.D', 'A.B.D' 等日志记录器所记录的事件。 但 'A.BB', 'B.A.B' 等则不允许。 如果用空字符串初始化，则所有事件都会通过。

## basicConfig()

可以设置输出文件, 输出格式，日期和logger level。如果不指定filemode为w则默认追加。

```python3
logging.basicConfig(filename="test.log", filemode="w", format="%(asctime)s %(name)s:%(levelname)s:%(message)s", datefmt="%d-%M-%Y %H:%M:%S", level=logging.DEBUG)
```

由 [`basicConfig()`](https://docs.python.org/zh-cn/3/library/logging.html#logging.basicConfig) 设置的默认format为：

```
severity:logger name:message
```

对 [`basicConfig()`](https://docs.python.org/zh-cn/3/library/logging.html#logging.basicConfig) 的调用应该在 [`debug()`](https://docs.python.org/zh-cn/3/library/logging.html#logging.debug)，[`info()`](https://docs.python.org/zh-cn/3/library/logging.html#logging.info) 等之前。否则，这些函数会替你用默认配置调用 [`basicConfig()`](https://docs.python.org/zh-cn/3/library/logging.html#logging.basicConfig)。它被设计为一次性的配置，只有第一次调用会进行操作，随后的调用不会产生有效操作。

## 配置日志记录

开发者可以通过三种方式配置日志记录：

1. 使用调用上面列出的配置方法的 Python 代码显式创建记录器、处理器和格式器。
2. 创建日志配置文件并使用 [`fileConfig()`](https://docs.python.org/zh-cn/3/library/logging.config.html#logging.config.fileConfig) 函数读取它。
3. 创建配置信息字典并将其传递给 [`dictConfig()`](https://docs.python.org/zh-cn/3/library/logging.config.html#logging.config.dictConfig) 函数。

```
import logging

# create logger
logger = logging.getLogger('simple_example')
logger.setLevel(logging.DEBUG)

# create console handler and set level to debug
ch = logging.StreamHandler()
ch.setLevel(logging.DEBUG)

# create formatter
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

# add formatter to ch
ch.setFormatter(formatter)

# add ch to logger
logger.addHandler(ch)

# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warning('warn message')
logger.error('error message')
logger.critical('critical message')
```
从配置文件出发：
```
[loggers]
keys=root,simpleExample

[handlers]
keys=consoleHandler

[formatters]
keys=simpleFormatter

[logger_root]
level=DEBUG
handlers=consoleHandler

[logger_simpleExample]
level=DEBUG
handlers=consoleHandler
qualname=simpleExample
propagate=0

[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=simpleFormatter
args=(sys.stdout,)

[formatter_simpleFormatter]
format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
```

代码
```
import logging
import logging.config

logging.config.fileConfig('logging.conf')

# create logger
logger = logging.getLogger('simpleExample')

# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warning('warn message')
logger.error('error message')
logger.critical('critical message')

```

### 警告

[`fileConfig()`](https://docs.python.org/zh-cn/3/library/logging.config.html#logging.config.fileConfig) 函数接受一个默认参数 `disable_existing_loggers` ，出于向后兼容的原因，默认为 `True` 。这可能与您的期望不同，因为除非在配置中明确命名它们（或其父级），**否则它将导致在 [`fileConfig()`](https://docs.python.org/zh-cn/3/library/logging.config.html#logging.config.fileConfig) 调用之前存在的任何非 root 记录器被禁用**。有关更多信息，请参阅参考文档，如果需要，请将此参数指定为 `False` 。

有尝试在notebook 使用logger，不好用，具体实验在server Playground/logTest