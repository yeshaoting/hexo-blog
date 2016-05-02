title: Python菜鸟教程学习笔记 - 条件语句
tags:
  - python
  - 学习笔记
categories:
  - 系统脚本
date: 2016-05-02 16:31:00
---

<img src="/asserts/images/logo/python.png" class="img-logo img-center" />


## 一、前言
python里的真为True，假为False，空为None.
Python程序语言指定任何非0和非空（None）条件判断结果为True，0 或者None条件判断结果为False。


## 二、条件判断形式
条件判断形式为：
``` python
if 判断条件1:
    执行语句1……
elif 判断条件2:
    执行语句2……
elif 判断条件3:
    执行语句3……
else:
    执行语句4……
```

**注**：别忘了if, elif, else关键字开头行尾的冒号。

<!-- more -->


## 三、实例
``` python
#!/usr/bin/env python
#coding=utf8

value=raw_input("请输入100以内分数：");

score=int(value);
if score > 90:
    print '优秀'
elif score > 80:
    print '良好'
elif score > 70:
    print '中等'
elif score > 60:
    print '及格'
else:
    print '不及格'
```

结果输出：
``` python
请输入100以内分数：20
不及格

请输入100以内分数：89
良好
```


## 四、补充
由于 python 并不支持 switch 语句，所以多个条件判断，只能用 elif 来实现，如果判断需要多个条件需同时判断时，可以使用 or （或），表示两个条件有一个成立时判断条件成功；使用 and （与）时，表示只有两个条件同时成立的情况下，判断条件才成功。


## 五、参考文档
[Python 条件语句](http://www.runoob.com/python/python-if-statement.html)

