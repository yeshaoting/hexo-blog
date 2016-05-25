title: 中止错误的多行SQL语句执行
tags:
  - mysql
categories:
  - 数据存储
date: 2016-05-25 20:36:00
---

<img src="/asserts/images/logo/mysql.png" class="img-logo img-center" />

## 一、背景
经常会出现在命令行编写多行SQL过程中，发现语法有问题。
此时，需要中止错误的SQL语句，取消SQL执行。


## 二、实例
编写多行SQL，中途发现语法错误，可以通过`\c`来中止。
如若SQL内容较多，可以通过`\p`打印出已经编写的SQL语句。

``` bash
mysql> select 123
    -> \p
--------------
select 123
--------------

    -> \c
```


## 三、扩展知识
更多mysql命令如下:
``` bash
List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.
```

## 四、参考文档
- [Mysql命令中断的问题 ](http://blog.sina.com.cn/s/blog_45ed26a7010002qi.html)
- [mysql命令行,多行命令时如何取消/返回修改前边的命令](https://fukun.org/archives/02291811.html)
