title: Python菜鸟教程学习笔记 - 数据类型
tags:
  - python
  - 学习笔记
categories:
  - 系统脚本
date: 2016-05-02 15:13:00
---

<img src="/asserts/images/logo/python.png" class="img-logo img-center" />


## 一、标准数据类型
Python有五个标准的数据类型：
- Numbers（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Dictionary（字典）


## 二、数字
Python支持四种不同的数字类型：
- int（有符号整型）
- long（长整型[也可以代表八进制和十六进制]）
- float（浮点型）
- complex（复数）

长整型定义在数值后添加一个小写或者大写的"L"，python中没有double类型数字。

使用完一个对象后，可通过del关键字删除对象引用。语法：
del var1[,var2[,var3[....,varN]]]]
``` python
value = 25
string = "yes"
del value, string
```

如若在删除对象引用还继续使用对象的话，则会报变量未定义的错误：`NameError: name 'value' is not defined`


<!-- more -->

## 三、字符串


## 四、列表


## 五、元组


## 六、字典


## 七、数据类型转换
| 函数 | 描述 |
|---|---|
| int(x [,base ])        | 将x转换为一个整数 |
| long(x [,base ])       | 将x转换为一个长整数 |
| float(x )              | 将x转换到一个浮点数 |
| complex(real [,imag ]) | 创建一个复数 |
| str(x)                | 将对象 x 转换为字符串 |
| repr(x)               | 将对象 x 转换为表达式字符串 |
| eval(str)             | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
| tuple(s)              | 将序列 s 转换为一个元组 |
| list(s)               | 将序列 s 转换为一个列表 |
| chr(x)                | 将一个整数转换为一个字符 |
| unichr(x)             | 将一个整数转换为Unicode字符 |
| ord(x)                | 将一个字符转换为它的整数值 |
| hex(x)                | 将一个整数转换为一个十六进制字符串 |
| oct(x)                | 将一个整数转换为一个八进制字符串 |

对于数值转换而言，如果字面量不是一个整数，会报错：`ValueError: invalid literal for int() with base 10: '1s`
正确实例：
``` python
>>> a="12"; long(a);
12L
>>> a="12"; int(a);
12
>>> a="12"; float(a);
12.0
```

对于元组，列表转换而言，如果字面量不可迭代，会报错：`TypeError: 'int' object is not iterable`
正确实例：
``` python
>>> a='12'; list(a);
['1', '2']
>>> a='12'; tuple(a);
('1', '2')
```


## 八、参考文档
- [Python 变量类型](http://www.runoob.com/python/python-variable-types.html)
- [Python Number(数字)](http://www.runoob.com//python/python-numbers.html)
- [Python 字符串](http://www.runoob.com//python/python-strings.html)
- [Python 列表(List)](http://www.runoob.com//python/python-lists.html)
- [Python 元组](http://www.runoob.com//python/python-tuples.html)
- [Python 字典(Dictionary)](http://www.runoob.com//python/python-dictionary.html)
