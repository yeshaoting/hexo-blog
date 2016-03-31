title: 了解IFS
tags:
  - 实践
  - shell
categories:
  - 脚本
date: 2016-02-01 17:37:00
---

## 一、介绍
内部字段分隔符IFS用作文本分隔符，用于将分隔文本成一个个词。默认的分隔符为空格、制表符以及换行。

通过命令`man bash`可以查看到bash的帮助文档，其中有一段内容如下：
> The Internal Field Separator that is used for word splitting after expansion and to  split lines into words with the read builtin command.
The default value is `<space><tab><newline>`.


## 二、相关知识

### 1. IFS内容查看
由于IFS默认包含的字符都是非可见字符，所以需要将字符内容转换成ASCII码或者八进制等查看。
``` bash
echo $IFS | od -a (Output named characters.)
echo $IFS | od -b (Output octal bytes.)
echo $IFS | od -c (Output C-style escaped characters.)
echo $IFS | od -x (Output hexadecimal bytes.)
```

### 2. 自定义IFS
一般情况，修改IFS只需给IFS这个全局变量赋值即可。
例如：设置默认分隔符为冒号`IFS=.`

特别地，对于空格、\n，\t等特殊字符或转义字符需要进行转义处理，如：`IFS=$'\n'`。


## 三、应用实例

### 1. 脚本
``` bash
#!/bin/bash

str="172.30.29.173"
arr=($str)
echo -e "默认分隔>> 长度：${#arr[@]} 内容：${arr[@]}"

IFS=.
arr=($str)
echo -e "冒号分隔>> 长度：${#arr[@]} 内容：${arr[@]}"

str="172 30.29 173"
IFS=$' '
arr=($str)
echo -e "空格分隔>> 长度：${#arr[@]} 内容：${arr[@]}"
```

### 2. 输出
``` bash
yerba-buena:tmp yeshaoting$ sh ifs.sh
默认分隔>> 长度：1 内容：172.30.29.173
冒号分隔>> 长度：4 内容：172 30 29 173
空格分隔>> 长度：3 内容：172 30.29 173
```


## 四、参考文档
1. [SHELL中的IFS详解](http://smilejay.com/2011/12/bash_ifs/)
2. [[Shell学习笔记] 内部字段分隔符IFS、脚本调试DEBUG](http://www.1987.name/205.html)
3. [Shell中的IFS解惑](http://blog.csdn.net/whuslei/article/details/7187639)
4. [What is the meaning of IFS=$'\n' in bash scripting ?](http://unix.stackexchange.com/questions/184863/what-is-the-meaning-of-ifs-n-in-bash-scripting)