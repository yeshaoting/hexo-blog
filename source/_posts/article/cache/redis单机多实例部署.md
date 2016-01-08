categories:
  - cache
title: redis单机多实例部署
tags:
  - redis
date: 2015-12-29 13:58:00
---

[TOC]

## 一、redis安装

## Redis介绍
Redis是一种高级key-value数据库。它跟memcached类似，不过数据可以持久化，而且支持的数据类型很丰富。有字符串，链表、哈希、集合和有序集合5种。支持在服务器端计算集合的并、交和补集(difference)等，还支持多种排序功能。所以Redis也可以被看成是一个数据结构服务器。Redis的所有数据都是保存在内存中，然后不定期的通过异步方式保存到磁盘上(这称为“半持久化模式”)；也可以把每一次数据变化都写入到一个append only file(aof)里面(这称为“全持久化模式”)。

### 1. 下载redis
``` shell
cd /opt/work/local

wget -c http://download.redis.io/releases/redis-3.0.3.tar.gz
tar -zxvf redis-3.0.3.tar.gz
```
解压后的文件目录如下：
![解压后的Redis目录结构](http://images.cnitblog.com/i/420264/201404/191557148074939.jpg)

### 2. 编译redis
``` shell
cd /opt/work/local/redis-3.0.3
make
```

### 3. 拷贝产生的可执行命令
``` shell
mkdir -p /opt/work/local/redis/bin
cd /opt/work/local/redis-3.0.3/src
cp -p redis-benchmark redis-check-aof redis-check-dump redis-cli redis-sentinel redis-server mkreleasehdr.sh /opt/work/local/redis/bin
```

### 4. redis服务启动
(1) 修改环境变量(vim  ~/.bash_profile)如下：
``` shell
REDIS_HOME=/opt/work/local/redis

PATH=$PATH:$HOME/bin:$REDIS_HOME/bin
```

(2) 修改完成后，记得使用`source ~/.bash_profile`生效。

(3) 查看redis-server使用文档
``` shell
[@zw_25_105 local]# redis-server --help
Usage: ./redis-server [/path/to/redis.conf] [options]
       ./redis-server - (read config from stdin)
       ./redis-server -v or --version
       ./redis-server -h or --help
       ./redis-server --test-memory <megabytes>

Examples:
       ./redis-server (run the server with default conf)
       ./redis-server /etc/redis/6379.conf
       ./redis-server --port 7777
       ./redis-server --port 7777 --slaveof 127.0.0.1 8888
       ./redis-server /etc/myredis.conf --loglevel verbose

Sentinel mode:
       ./redis-server /etc/sentinel.conf --sentinel
```

(4) 启动单机redis
使用默认的参数启动redis：`redis-server`
指定端口启动redis：`redis-server --port 6380`

(5) 允许redis端口远程连接
修改防火墙配置文件，如下所示：
``` shell
vim /etc/sysconfig/iptables

# 添加一行
-A INPUT -m state --state NEW -m tcp -p tcp --dport 6380 -j ACCEPT

# 重新加载规则
service iptables restart
```

至此你就可以用客户端redis-cli连接了：`redis-cli -h 127.0.0.1 -p 6380`

<!-- more -->

## 二、redis主从配置


## 三、redis sentinel配置





## 四、参考文档：
[Redis安装及主从配置](!http://www.cnblogs.com/liuling/p/2014-4-19-02.html)
[centos 安装redis（一台机器可以安装多个redis）](!http://www.cnblogs.com/eric-z/p/4153101.html)
[在一台机器上搭建多个redis实例](!http://my.oschina.net/liuke1556/blog/287594)
