title: 升级mac bash到4.3版本
tags:
  - mac
  - shell
categories:
  - mac
date: 2016-01-29 18:52:00
---

# 升级mac bash到4.3版本


## 一、前言
MAC OS版本：OS X Yosemite 10.10.5

经查看bash版本发现，默认的bash版本为：3.2.57(1)-release
``` bash
yerba-buena:~ yeshaoting$ /bin/bash -version
GNU bash, version 3.2.57(1)-release (x86_64-apple-darwin14)
Copyright (C) 2007 Free Software Foundation, Inc.
```


## 二、版本升级
借助mac下的命令行安装工具brew安装bash。

### 方式1：brew安装
``` bash
yerba-buena:article yeshaoting$ brew install bash
==> Installing dependencies for bash: readline
==> Installing bash dependency: readline
==> Downloading https://homebrew.bintray.com/bottles/readline-6.3.8.yosemite.bottle.tar.gz
……
```

### 方式2：自行编译安装
bash下载地址：http://ftp.gnu.org/gnu/bash/

```
wget http://ftp.gnu.org/gnu/bash/bash-4.2.tar.gz
tar zxvf bash-4.2.tar.gz
cd bash-4.2
./configure
make
make install
```


## 三、版本替换

### 1. 替换bash
``` bash
yerba-buena:~ yeshaoting$ sudo mv /bin/bash /bin/bash.origin
yerba-buena:~ yeshaoting$ sudo ln -s /usr/local/opt/bash/bin/bash /bin/bash
```

由于本mac版本下的sh并不是直接链接bash，而是bash的一个文件拷贝。因此，还要替换默认的sh指向的命令为bash，如下：
``` bash
yerba-buena:~ yeshaoting$ sudo mv /bin/bash /bin/sh.origin
yerba-buena:~ yeshaoting$ sudo ln -s /usr/local/opt/bash/bin/bash /bin/sh
```

### 2. 验证
通过bash和sh命令查看版本：
``` bash
yerba-buena:~ yeshaoting$ bash -version
GNU bash，版本 4.3.42(1)-release (x86_64-apple-darwin14.5.0)
Copyright (C) 2013 Free Software Foundation, Inc.
许可证 GPLv3+: GNU GPL 许可证第三版或者更新版本 <http://gnu.org/licenses/gpl.html>

This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

通过BASH_VERSION变更查看版本：
``` bash
yerba-buena:~ yeshaoting$ echo $BASH_VERSION
4.3.42(1)-release
```


## 四、重要特性
关联数组从bash 4.0开始被引入，关联数组的索引值可以使用任意的文本。


## 五、参考文档
1. [升级Mountain Lion的bash到4.2版本](http://zengrong.net/post/1830.htm)
2. [[Shell学习笔记] 数组、关联数组和别名使用](http://www.1987.name/164.html)
3. [shell升级，Linux系统升级bash](http://www.1987.name/166.html)
4. [MAC系统默认命令行环境BASH版本升级修改](http://levi.yii.so/archives/4862)