title: Python菜鸟教程学习笔记 - 循环语句
tags:
  - python
  - 学习笔记
categories:
  - 系统脚本
---

<img src="/asserts/images/logo/python.png" title="python" class="img-logo img-center" />


## 一、概述
程序在一般情况下是按顺序执行的。
编程语言提供了各种控制结构，允许更复杂的执行路径。
本章要介绍的循环语句，允许我们执行一个语句或语句组多次。


### 1. 循环类型
| 循环类型 | 描述 |
|---|---|
| while循环 | 在给定的判断条件为 true 时执行循环体，否则退出循环体。 |
| for循环 | 重复执行语句 |
| 嵌套循环	| 你可以在while循环体中嵌套for循环 |

### 2. 循环控制
| 控制语句 | 描述 |
|---|---|
| break 语句 | 在语句块执行过程中终止循环，并且跳出整个循环。 |
| continue 语句 | 在语句块执行过程中终止当前循环，跳出该次循环，执行下一次循环。 |
| pass 语句 | pass是空语句，是为了保持程序结构的完整性。 |


## 二、while循环
Python 编程中 while 语句用于循环执行程序，即在某条件下，循环执行某段程序，以处理需要重复处理的相同任务。

### 1. 语法格式
> while 判断条件：
    执行语句……

### 2. 实例
``` python
#!/usr/bin/env python
#coding=utf8

count = 0;
while count < 9:
    print 'The count is :', count
    count = count + 1;

print "Good bye!"
```

<!-- more -->

### 3. 输出结果
``` bash
yerba-buena:loop yeshaoting$ python while.py
The count is : 0
The count is : 1
The count is : 2
The count is : 3
The count is : 4
The count is : 5
The count is : 6
The count is : 7
The count is : 8
Good bye!
```


## 三、for循环
Python for循环可以遍历任何序列的项目，如一个列表或者一个字符串。

### 1. 语法格式
> for iterating_var in sequence:
   statements(s)

### 2. 实例
``` python
#!/usr/bin/env python
#coding=utf8

string = 'Python'
for letter in string:
    print letter;


for letter in range(len(string)):
    print letter;


fruits = ['apple', 'mango', 'banana']
for fruit in fruits:
    print fruit;
```

### 3. 扩展知识

#### 内置函数len
返回序列(string, bytes, tuple, list, range)长度。
> Return the length (the number of items) of an object. The argument may be a sequence (such as a string, bytes, tuple, list, or range) or a collection (such as a dictionary, set, or frozen set).

#### 内置函数range
range返回一个序列的数。

有两种用法：
range(stop)
range(start, stop[, step])


### 4. 输出结果
``` bash
yerba-buena:loop yeshaoting$ python for.py
P
y
t
h
o
n
0
1
2
3
4
5
apple
mango
banana
```


## 四、嵌套循环
Python 语言允许在一个循环体里面嵌入另一个循环。


## 五、循环控制
break语句用来终止循环语句，即循环条件没有False条件或者序列还没被完全递归完，也会停止执行循环语句。
continue 语句用来告诉Python跳过当前循环的剩余语句，然后继续进行下一轮循环。
pass 不做任何事情，一般用做占位语句。


## 六、参考文档
- [Python 循环语句](http://www.runoob.com//python/python-loops.html)
- [Python While循环语句](http://www.runoob.com//python/python-while-loop.html)
- [Python for 循环语句](http://www.runoob.com//python/python-for-loop.html)
- [Python 循环嵌套](http://www.runoob.com//python/python-nested-loops.html)
- [Python break 语句](http://www.runoob.com//python/python-break-statement.html)
- [Python continue  语句](http://www.runoob.com//python/python-continue-statement.html)
- [Python pass 语句](http://www.runoob.com//python/python-pass-statement.html)