title: shell关联数组基本用法
tags:
  - 实践
  - shell
categories:
  - 脚本
date: 2016-02-01 12:35:00
---
# shell关联数组基本用法

[TOC]

**注**：bash自版本4后才开始支持关联数组，之前的版本可以考虑通过二维数组来代替。


## 一、基本用法

### 1. 数组声明
``` bash
declare -a arr
```


### 2. 数组赋值

#### 静态赋值
``` bash
arr=([A]=A1, [C]=C2, [E]=E3, [B]=B411, [b]=b5)
```

#### 动态赋值
``` bash
arr["A"]=A1
arr["C"]=C2
arr["E"]=E3
arr["B"]=B411
arr["b"]=b5
```


### 3. 数组遍历

#### 数组索引(键)
格式：`${!arr[@]}` 或 `${!arr[*]}`

#### 数组内容(值)
格式：`${arr[@]}` 或 `${arr[*]}`


### 4. 数组长度
格式：
``` bash
${#变量}
```
该变量若为数组，则为数组长度；若为普通变量，则为普通变量长度。

数组长度：
``` bash
${#arr[@]}
${#arr[*]}
```
数组元素长度：
``` bash
${#arr[2]}
```


### 5. 子数组
格式：
``` bash
${#arr:index:end}
```


<!-- more -->


## 二、举例

### 1. 实例脚本
``` bash
#/bin/bash

declare -A arr

arr["A"]=A1
arr["C"]=C2
arr["E"]=E3
arr["B"]=B411
arr["b"]=b5


# 数组长度
echo -e '数组长度${#arr[@]}:\t'${#arr[@]};
echo -e '数组长度${#arr[@]:1:4}:\t'${#arr[@]:1:4};
echo -e '单个元素长度${#arr[C]}:\t'${#arr[C]};
echo -e '单个元素长度${#arr[B]}:\t'${#arr[B]};
echo -e '';

# 数组
echo -e '数组索引${!arr[@]}:\t'${!arr[@]};
echo -e '全部数组内容${arr[@]}:\t'${arr[@]};
echo -e '';

# 数组子串
echo -e '数组子串${arr[@]:2}:\t'${arr[@]:2};
echo -e '数组子串${arr[@]:1:4}:\t'${arr[@]:1:4};
echo -e '';

# 数组值
echo -e '数组值${arr[b]}:\t'${arr[b]};
echo -e '数组值${arr[C]}:\t'${arr[C]};
echo -e '数组值(大小写敏感)${arr[c]}:\t'${arr[c]};
```


### 2. 实例输出
``` bash
数组长度${#arr[@]}:	5
数组长度${#arr[@]:1:4}:	5
单个元素长度${#arr[C]}:	2
单个元素长度${#arr[B]}:	4

数组索引${!arr[@]}:	A B C E b
全部数组内容${arr[@]}:	A1 B411 C2 E3 b5

数组子串${arr[@]:2}:	B411 C2 E3 b5
数组子串${arr[@]:1:4}:	A1 B411 C2 E3

数组值${arr[b]}:	b5
数组值${arr[C]}:	C2
数组值(大小写敏感)${arr[c]}:
```


## 三、附加知识

### 1. 内部域分隔符IFS
shell里默认使用空格、制表符以及换行为分隔符。

### 2. 数组转换
字符串可通过()默认通过IFS将字符串直接转换成数组。
``` bash
yerba-buena:tmp yeshaoting$ str="1 3 2 7 5"
yerba-buena:tmp yeshaoting$ arr=($str)
yerba-buena:tmp yeshaoting$ echo ${arr[@]}
1 3 2 7 5
yerba-buena:tmp yeshaoting$ echo ${!arr[@]}
0 1 2 3 4
```

### 3. 数组键大小写敏感
`${arr[C]}`取值为C2，`${arr[c]}`则为空。
另外，键加不加引号都一样，引号可用于拼接键值操作。

### 4. 关联数组键有序
关联数组依键有序，与插入数组顺序无关。如上例中数组输出结果为：A1 B411 C2 E3 b5。

### 5. 索引负值
数组索引负值表示从数组末尾输出。其中，-1表示数组最后一个元素，-max_num-1表示数组第一个元素。
如果索引值index<-max_num-1，则会报“数组下标不正确”的异常。
``` bash
yerba-buena:tmp yeshaoting$ arr=(1 3 2 7 5)
yerba-buena:tmp yeshaoting$ echo ${arr[-1]}
5
yerba-buena:tmp yeshaoting$ echo ${arr[-5]}
1
yerba-buena:tmp yeshaoting$ echo ${arr[-6]}
-bash: arr: 数组下标不正确
```

### 6. 索引超过数组最大个数
不输出任何内容，也不会报错。
``` bash
yerba-buena:tmp yeshaoting$ arr=(1 3 2 7 5)
yerba-buena:tmp yeshaoting$ echo ${arr[6]}
```



## 四、参考文档
1. [shell 数组应用实例](http://salogs.com/news/2015/08/02/shell-array-demo/)
2. [[Shell学习笔记] 数组、关联数组和别名使用](http://www.1987.name/164.html)