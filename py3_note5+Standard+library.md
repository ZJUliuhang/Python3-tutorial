
#### OS接口

* dir()是py内置函数，可以查看对象或者模块内的方法
* help() 是py内置函数，可以查看对象或者模块内的方法的详细说明

* os模块提供了与操作系统相关联的函数
* shutil 模块提供了一个**易于使用**的高级接口


```python
#import numpy  建议使用 "import os" 风格而非 "from os import *"。这样可以保证随操
#作系统不同而有所变化的 os.open() 不会覆盖内置函数 open()
#dir(numpy) dir('') dir([]),也可以放入变量名，返回对应的操作方法
#help(numpy)  help(dir) help([]) help('')
a=[]
help(a.append)
```

    Help on built-in function append:
    
    append(...) method of builtins.list instance
        L.append(object) -> None -- append object to end
    
    

#### 文件通配符


```python
import glob
glob.glob('*.ipynb')生成所在目录的文件列表
```


      File "<ipython-input-2-6e24ca31c2d1>", line 2
        glob.glob('*.ipynb')生成所在目录的文件列表
                                      ^
    SyntaxError: invalid syntax
    


#### 字符串正则匹配

* re模块为高级字符串处理提供了正则表达式工具。对于复杂的匹配和处理，正则表达式提供了简洁、优化的解决方案
* 如果只需要简单的功能，应该首先考虑字符串方法，因为它们非常简单，易于阅读和调试:

#### 访问互联网


* 有几个模块用于访问互联网以及处理网络通信协议。其中最简单的两个是用于处理从 urls 接收的数据的 urllib.request 以及用于发送电子邮件的 smtplib


```python
##运行失败
import smtplib
server = smtplib.SMTP('localhost')
server.sendmail('liuhangjiayuan2010@163.com', '1972259230@qq.com',
                """To: 1972259230@qq.com
                From: liuhangjiayuan2010@163.com
                Beware the Ides of March.
                """)
server.quit()
#这需要本地有一个在运行的邮件服务器。
```


    ---------------------------------------------------------------------------

    ConnectionRefusedError                    Traceback (most recent call last)

    <ipython-input-3-ef3b04a6e007> in <module>()
          1 ##运行失败
          2 import smtplib
    ----> 3 server = smtplib.SMTP('localhost')
          4 server.sendmail('liuhangjiayuan2010@163.com', '1972259230@qq.com',
          5                 """To: 1972259230@qq.com
    

    D:\Anaconda3\lib\smtplib.py in __init__(self, host, port, local_hostname, timeout, source_address)
        249 
        250         if host:
    --> 251             (code, msg) = self.connect(host, port)
        252             if code != 220:
        253                 self.close()
    

    D:\Anaconda3\lib\smtplib.py in connect(self, host, port, source_address)
        334         if self.debuglevel > 0:
        335             self._print_debug('connect:', (host, port))
    --> 336         self.sock = self._get_socket(host, port, self.timeout)
        337         self.file = None
        338         (code, msg) = self.getreply()
    

    D:\Anaconda3\lib\smtplib.py in _get_socket(self, host, port, timeout)
        305             self._print_debug('connect: to', (host, port), self.source_address)
        306         return socket.create_connection((host, port), timeout,
    --> 307                                         self.source_address)
        308 
        309     def connect(self, host='localhost', port=0, source_address=None):
    

    D:\Anaconda3\lib\socket.py in create_connection(address, timeout, source_address)
        722 
        723     if err is not None:
    --> 724         raise err
        725     else:
        726         raise error("getaddrinfo returns an empty list")
    

    D:\Anaconda3\lib\socket.py in create_connection(address, timeout, source_address)
        711             if source_address:
        712                 sock.bind(source_address)
    --> 713             sock.connect(sa)
        714             # Break explicitly a reference cycle
        715             err = None
    

    ConnectionRefusedError: [WinError 10061] 由于目标计算机积极拒绝，无法连接。


#### 日期和时间

* datetime模块为日期和时间处理同时提供了简单和复杂的方法。
* 支持日期和时间算法的同时，实现的重点放在更有效的处理和格式化输出。
* 该模块还支持时区处理


```python
from datetime import date
print(date.today())
now=date.today()
a=now.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")
#将AB换成小写后悔按照缩写输出
print(a)
```

    2017-12-25
    12-25-17. 25 Dec 2017 is a Monday on the 25 day of December.
    

#### 数据压缩

* 以下模块直接支持通用的数据打包和压缩格式：zlib，gzip，bz2，zipfile，以及 tarfile


```python
import zlib
s = b'witch which has which witches wrist watch'
print(len(s))
t = zlib.compress(s)
print(len(t))
print(zlib.decompress(t))
print(zlib.crc32(s))
```

    41
    37
    b'witch which has which witches wrist watch'
    226805979
    

#### 性能度量

* python提供一个度量工具，了解解决同一问题的不同方法之间的性能差异。

* 相对于 timeit 的细粒度，:mod:profile 和 pstats 模块提供了针对更大代码块的时间度量工具

#### 测试模块

* doctest模块提供了一个工具，扫描模块并根据程序中内嵌的文档字符串执行测试。
* unittest模块不像 doctest模块那么容易使用，不过它可以在一个独立的文件里提供一个更全面的测试集


```python
def average(values):
    """Computes the arithmetic mean of a list of numbers.

    >>> print(average([20, 30, 70]))
    40.0
    """
    return sum(values) / len(values)

import doctest
doctest.testmod()   # 自动验证嵌入测试
```




    TestResults(failed=0, attempted=1)




```python
import unittest

class TestStatisticalFunctions(unittest.TestCase):

    def test_average(self):
        self.assertEqual(average([20, 30, 70]), 40.0)
        self.assertEqual(round(average([1, 5, 7]), 1), 4.3)
        self.assertRaises(ZeroDivisionError, average, [])
        self.assertRaises(TypeError, average, 20, 30, 70)

unittest.main() # Calling from the command line invokes all tests
```

    E
    ======================================================================
    ERROR: C:\Users\HANG LIU\AppData\Roaming\jupyter\runtime\kernel-e4ccf42d-9a61-40a0-ad08-102b122476de (unittest.loader._FailedTest)
    ----------------------------------------------------------------------
    AttributeError: module '__main__' has no attribute 'C:\Users\HANG LIU\AppData\Roaming\jupyter\runtime\kernel-e4ccf42d-9a61-40a0-ad08-102b122476de'
    
    ----------------------------------------------------------------------
    Ran 1 test in 0.002s
    
    FAILED (errors=1)
    


    An exception has occurred, use %tb to see the full traceback.
    

    SystemExit: True
    


    D:\Anaconda3\lib\site-packages\IPython\core\interactiveshell.py:2870: UserWarning: To exit: use 'exit', 'quit', or Ctrl-D.
      warn("To exit: use 'exit', 'quit', or Ctrl-D.", stacklevel=1)
    
