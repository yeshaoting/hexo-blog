title: python中文乱码
tags:
  - python
categories:
  - 系统脚本
date: 2016-05-25 20:33:00
---
<img src="/asserts/images/logo/python.png" class="img-logo img-center" />

本文内容基于python2.7而言，python3不在讨论范围内。

## 一、背景
在python文件中定义一个包含中文的字符串，打印到console不会出现乱码，但是将内容输出到文件里时，报如下异常：
```
Traceback (most recent call last):
  File "encoding.py", line 24, in <module>
    dumpFile(s);
  File "encoding.py", line 8, in dumpFile
    result = result.encode('utf-8')
UnicodeDecodeError: 'ascii' codec can't decode byte 0xe8 in position 0: ordinal not in range(128)
```


## 二、python字符编码
Python里面有两种数据模型来支持字符串这种数据类型，一种是str，另外一种是unicode，它们都是sequence 的派生类。
str：是指带有编码的字符串；unicode：是指不带编码的字符串。

### 1. 编码: unicode -> str
unicode编码的字符串可以通过encode(charset)函数，转成str字符串。

### 2. 解码: str -> unicode
str字符串也可以通过decode(charset)函数，转成unicode字符串。

### 3. 附加知识
isinstance()：用于判断对象是否某个类型
`__class__`：python对象内置属性，用于查看对象所属的类型。
type(): 查看对象类型。


## 三、编码

### 1. 源文件编码
python2.7默认脚本文件编码方式为ASCII，如果代码中出现非ASCII编码的字符则会报错。
可以采用如下方式指定脚本中出现字符串的编码方式:
```
# -*- coding: utf-8 -*-
# coding=utf-8
```

### 2. 内部转换编码
python内部数据存储编码格式是unicode编码。
对于代码文件中出现的非unicode编码的字符串，python会统一转成unicode编码存储。

对于控制台字符串输出，
对于文件导出或网络数据传输，由于python并不知道需要保存

```
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
```

## 五、检查字符编码方式
``` python
def encoding(s):
    cl = [''utf8'', ''gb2312'']
    for a in cl:
        try:
            s.decode(a)
            return a
        except UnicodeEncodeError:
            pass
    return ''unknown''
```


## 六、参考文档
- [也谈 Python 的中文编码处理](http://in355hz.iteye.com/blog/1860787)
- [【转】Python 编码问题整理  ](http://blog.163.com/sea_haitao/blog/static/775621620096412211732/)
- [python 中文乱码问题深入分析](http://www.jb51.net/article/26543.htm)

