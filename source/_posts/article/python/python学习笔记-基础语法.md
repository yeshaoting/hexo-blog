title: Python菜鸟教程学习笔记 - 基础语法
tags:
  - python
  - 学习笔记
categories:
  - 系统脚本
date: 2016-05-02 15:13:00
---

<img src="/asserts/images/logo/python.png" class="img-logo img-center" />


## 一、第一个Python程序

### 1. 交互式编程
在mac或者linux终端输入python命令即可进入交互式编程。如：
``` python
yerba-buena:pystudy yeshaoting$ python
Python 2.7.10 (default, Jul 14 2015, 19:46:27)
[GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.39)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

### 2. 脚本式编程
将python代码写入一个以.py为扩展名的python文件，如：test.py。
脚本写完后，可以通过如下二种方式执行python脚本：python test.py。

若python文件里设置了python shebang，则可以通过./test.py来执行。


## 二、python标识符
> 在python里，标识符有字母、数字、下划线组成。
在python中，所有标识符可以包括英文、数字以及下划线（_），但不能以数字开头。
python中的标识符是区分大小写的。
以下划线开头的标识符是有特殊意义的。以单下划线开头（_foo）的代表不能直接访问的类属性，需通过类提供的接口进行访问，不能用"from xxx import *"而导入；
以双下划线开头的（__foo）代表类的私有成员；以双下划线开头和结尾的（__foo__）代表python里特殊方法专用的标识，如__init__（）代表类的构造函数。

## 三、Python保留字符
| and | exec | not |
|:---:|:---:|:---:|
| assert | finally | or |
| break | for | pass |
| class | from | print |
| continue | global | raise |
| def | if | return |
| del | import | try |
| elif | in | while |
| else | is | with |
| except | lambda | yield |


<!-- more -->

## 四、行和缩进
> 学习Python与其他语言最大的区别就是，Python的代码块不使用大括号（{}）来控制类，函数以及其他逻辑判断。python最具特色的就是用缩进来写模块。
缩进的空白数量是可变的，但是所有代码块语句必须包含相同的缩进空白数量，这个必须严格执行。
IndentationError: unexpected indent 错误是python编译器是在告诉你"Hi，老兄，你的文件里格式不对了，可能是tab和空格没对齐的问题"，所有python对格式要求非常严格。
如果是 IndentationError: unindent does not match any outer indentation level错误表明，你使用的缩进方式不一致，有的是 tab 键缩进，有的是空格缩进，改为一致即可。

**个人建议**：因为不同的系统、不同的IDE tab缩进设置长度不一，建议使用空格代替tab缩进，能省去一些不必要的麻烦。


## 五、多行语句
语句中包含[], {} 或 () 括号就不需要使用多行连接符。如：python列表、元组、字典定义。
而对于语句非如上三类情况，可以在换行处使用斜杆( **\** )，达到换行目的。例如：
```
#!/usr/bin/env python
#coding=utf8

count = 0
count = count + 1 \
    + 3 + 5;

print '总计：', count;
```

如果行尾未使用斜杆的话，可能会报非法缩进异常：`IndentationError: unexpected indent`。


## 五、python引号
python里的引号有三种形式：单引号(')，双引号(")以及三引号(''' 或 """)。
其中，单引号和双引号都用于定义一个字符串变量，而三引号可以用来定义一个段落(包含换行)变量。例如：
``` python
paragraph = """这是一个段落。
包含了多个语句"""

print paragraph
```

另外，由于在python里字符串可以单独存在，而不赋值给任何变量。这种形式字符串可以被当作注释使用。例如：
``` python
'''
Created on Apr 27, 2016

@author: yeshaoting
'''
```

## 六、python注释
单行注释：`#`
多行注释：使用三引号。参见：python引号


## 七、接收用户输入
内建函数：raw_input，用于接收终端用户输入，返回一个字符串。例如：
``` python
value=raw_input("input:");
print "user input value:", value
print "user input value type:", type(value)
```

执行实例为：
``` python
input:1
user input value: 1
user input value type: <type 'str'>

input:a b ef
user input value: a b ef
user input value type: <type 'str'>
```


## 八、多个语句构成代码块
像if、while、def和class这样的复合语句，代码块首行以关键字开始，需要以冒号(:)结束。
后面的子句使用相同的缩进用以表示同属一个代码块。

一个代码块结束后建议添加一个空白行使用代码块间界线分明，容易分辨。


## 九、附录
参考文档：[Python基础语法](http://www.runoob.com/python/python-basic-syntax.html)
扩展阅读：[Python 命令行参数](http://www.runoob.com/python/python-command-line-arguments.html)