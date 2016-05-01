title: python打印中文异常
tags:
  - python
categories:
  - 系统脚本
date: 2016-05-01 19:46:00
---

<img src="/asserts/images/logo/python.png" class="img-logo img-center" />


## 一、背景
脚本里需要打印中文内容。


## 二、脚本
``` python
'''
Created on Apr 27, 2016

@author: yeshaoting
'''

if __name__ == '__main__':
    print '你好，python!'
```


## 三、问题
python脚本里包含有中文时，报如下错误：
``` bash
Traceback (most recent call last):
  File "/Applications/STS.app/Contents/Eclipse/plugins/org.python.pydev_4.5.5.201603221110/pysrc/pydevd.py", line 1529, in <module>
    globals = debugger.run(setup['file'], None, None, is_module)
  File "/Applications/STS.app/Contents/Eclipse/plugins/org.python.pydev_4.5.5.201603221110/pysrc/pydevd.py", line 936, in run
    pydev_imports.execfile(file, globals, locals)  # execute the script
  File "/Users/yeshaoting/java/workspace/python-workspace/HelloPython/base/HelloWorld.py", line 8
SyntaxError: Non-ASCII character '\xe4' in file /Users/yeshaoting/java/workspace/python-workspace/HelloPython/base/HelloWorld.py on line 8, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
```

<!-- more -->

## 四、解决方法
告诉python解释器，按照utf-8编码读取源代码。

具体做法：在python脚本开头添加如下内容：
``` python
# -*- coding: utf-8 -*-
```

## 五、参考文档
[也谈 Python 的中文编码处理](http://in355hz.iteye.com/blog/1860787)