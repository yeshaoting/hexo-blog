title: MySQL监控工具-Innotop
date: 2016-08-16 14:57:51
---

## 一、简介
InnoTop是一个系统活动报告，类似于Linux性能工具，它与Linux的top命令相仿，并参考mytop工具而设计。
它专门用于监控InnoDB性能和MySQL服务器，主要内容有：监控事务，死锁，外键错误，查询活动，复制活动，系统变量的主要统计信息及主机的其他详情。
InnoTop被广泛使用，并被当做常用性能监控工具。


## 二、工具安装

### 1. 使用brew安装
**注**：just for mac

#### 安装brew
``` bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

通过brew安装的软件会放在 /usr/local/Cellar 目录，并将相关工具链接到 /usr/local/bin 目录。

#### 安装innotop
``` bash
brew install innotop
```

### 2. 源码编译安装


## 三、应用



## 参考文档
1. [mysql监控管理工具--innotop](http://blog.csdn.net/wyzxg/article/details/8609981)

