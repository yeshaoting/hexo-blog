title: python中文乱码
tags:
  - python
categories:
  - 系统脚本
date: 2016-05-25 20:33:00
---
<img src="/asserts/images/logo/python.png" class="img-logo img-center" />

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


## 二、探索与学习

### 1. 前提知识
isinstance：用于判断对象是否某个类型
__class__：python对象内置属性，用于查看对象所属的类型。

### 2. python字符编码
Python里面有两种数据模型来支持字符串这种数据类型，一种是str，另外一种是unicode，它们都是sequence 的派生类。
str：是指带有编码的字符串；unicode：是指不带编码的字符串。


[也谈 Python 的中文编码处理](http://in355hz.iteye.com/blog/1860787)