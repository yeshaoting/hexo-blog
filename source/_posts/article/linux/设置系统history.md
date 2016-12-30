title: 设置系统history
tags:
  - linux
categories:
  - 系统配置
author: yeshaoting
date: 2016-08-27 19:17:00
---

# 设置系统history



## 一、history 记录设置



### 1. 历史记录增加时间：HISTTIMEFORMAT

```shell
HISTTIMEFORMAT='%F %T '     #注意有个空格，为了显示时日期与命令之间有空格分割。
```



### 2. 历史命令记录显示行数：HISTSIZE

```bash
HISTSIZE=3000
HISTFILESIZE=3000
```



### 3. 历史文件名称更改：HISTFILE 

```shell
HISTFILE=/var/history/$USER-$UID.log
```



### 4. 剔除连续重复的条目：HISTCONTROL

```shell
HISTCONTROL=ignoredups  //保存退出
```



### 5. 多终端共享history

``` shell
shopt -s histappend
```

由于bash的history文件默认是覆盖，如果存在多个终端，最后退出的会覆盖以前历史记录，改为追加形式。

### 6. 实时追加history

``` shell
PROMPT_COMMAND="history -a"
```

实时追加当前session历史命令记录到history文件。

**注**：`history -w`则是会用当前session的历史命令替换history文件，而history -w则是会用当前session的历史命令替换history文件。



## 二、定期清理和保留 history 记录

``` shell
#!/bin/bash
# archive linux command history files
# written by vpsee.com

umask 077
maxlines=2000

lines=$(wc -l < ~/.bash_history)

if (($lines > $maxlines)); then
    cut=$(($lines - $maxlines))
    head -$cut ~/.bash_history >> ~/.bash_history.sav
    sed -e "1,${cut}d"  ~/.bash_history > ~/.bash_history.tmp
    mv ~/.bash_history.tmp ~/.bash_history
fi
```


<!-- more -->


## 三、参考文档

1. [设置linux系统history相关变量，命令时间、保存history条数，多session共享history](http://coolnull.com/4013.html)
2. [[译] 如何防止丢失任何 bash 历史命令?](http://blog.felixc.at/2013/09/how-to-avoid-losing-any-history-lines/)
3. [定期清理和保留 history 记录](http://www.vpsee.com/2011/08/archiving-linux-commands-history/)