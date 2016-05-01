tags:
  - python
categories:
  - 系统脚本
title: 《跟老齐学Python》学习笔记-数
date: 2016-05-01 22:34:00
---
<img src="/asserts/images/logo/python.png" class="img-logo img-center" />

**注**：所有示例基于python2。
**注**：由于老齐的教程面向零基础，简单通俗适用于编程新手；其篇幅又过长，很不适合已经熟知其他编程语言的人员使用，后期打算换用其他的教程进行学习。

## 一、数和四则运算

### 1. 查看对象内存地址：id()
``` python
>>> id(8)
140200127779760
>>> id(23.56)
140200127787712
```

### 2. 查看对象类型：type()
``` python
>>> type(2)
<type 'int'>
>>> type(3.4)
<type 'float'>
>>> type(23333333333333333333)
<type 'long'>
>>> type('yeshaoting')
<type 'str'>
```

### 3. 对象有类型，变量无类型
python中变量仅仅只一个标识，指向某个存储空间。
程序执行赛程中其类型可以任意修改。
相对于java强类型言语而言，python是弱类型的。
``` python
>>> a=2; type(a); a=3.4; type(a);
<type 'int'>
<type 'float'>
```

### 4. 浮点数与整数运算结果是浮点数。

### 5. 运算溢出问题
整数间运算不会出现整数溢出问题，但是如果与浮点数一起计算就有可能出现溢出问题。

参见：https://github.com/yeshaoting/StarterLearningPython/blob/master/102.md


## 二、除法

### 1. 整型相除
整型相除结果是商(结果不会四舍五入)，不会出现小数。例如：
``` python
>>> 4/5
0
```

### 2. 浮点数运算结果是浮点数，不会出现四舍五入的情况。

### 3. 引用division模块解决除法问题
division模块重新定义了除法的一些特性，如：整数相除得到的结果是一个浮点数。
``` python
>>> from __future__ import division
>>> 3/4
0.75
>>> 2/3
0.6666666666666666
```

### 4. 取余
取余数可使用`%`，例如：
``` python
>>>> 4%5
4
```

### 4. 内建函数divmod
python提供的内建函数divmod也能为除法运算一次性返回商和余数。例如：
``` python
>>> divmod(2, 3)
(0, 2)
>>> divmod(3, 1)
(3, 0)
>>> divmod(3, 2)
(1, 1)
```


### 5. 四舍五入
内建函数round() 方法返回浮点数x的四舍五入值。

``` python
>>> round(2, 3)
2.0
>>> round(2/3, 1)
0.7
>>> round(2/3, 0)
1.0
>>> round(2/4, 0)
1.0
```

参见：https://github.com/yeshaoting/StarterLearningPython/blob/master/103.md

