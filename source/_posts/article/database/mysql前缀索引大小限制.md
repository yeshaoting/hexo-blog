title: mysql前缀索引大小限制
tags:
  - mysql
  - 索引
categories:
  - 数据存储
date: 2016-08-04 11:06:00
---
<img src="/asserts/images/logo/mysql.png" class="img-logo img-center" />


## 一、背景
更改数据表存储引擎由MyISAM修改为INNODB，提示如下错误：
ERROR 1071: Specified key was too long; max key length is 767 bytes


## 二、问题描述
今有一张数据表如下，其结构如下：
``` sql
CREATE TABLE IF NOT EXISTS `base_info`
(
  `id` bigint(20) NOT NULL DEFAULT '0' COMMENT 'ID',
  `name` varchar(256) DEFAULT NULL COMMENT '名称',
  `urls` varchar(1000) DEFAULT NULL COMMENT '链接',
  `partition_month` varchar(256) NOT NULL DEFAULT '' COMMENT '快照月份',
  PRIMARY KEY (`id`,`partition_month`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 COMMENT='基础信息表';
```

由于基于此表可能会用到事务。因此，MyISAM不满足需求，需要将表ENGINE修改为INNODB。
具体SQL语句如下：
``` sql
ALTER TABLE `base_info`
ENGINE = InnoDB;
```


## 三、解决方案

### 1. 原因分析
不同的数据表存储引擎限制的索引长度不一。
数据表存储引擎MyISAM最大索引长度为 **1000B**，而INNODB默认最大索引长度为**767B**。

上述问题涉及的primary key中partition_month长度为 **256 * 3B = 768B > 767B**。

### 2. 问题处理
修改索引字段长度，并指定用于索引建立的前缀长度。
目前能想到的问题处理方案有如下两种：

#### 基于业务场景
根据业务需求调整不合理的字段类型或者长度。本案例就属于这类情况。

从业务角度来看，partition_month值形如：201607。
因此，可以将字段类型调整长度为int类型或者将partition_month字符长度设置成 char(6) 即可。
``` sql
ALTER TABLE `test`.`base_info`
DROP PRIMARY KEY,
ADD PRIMARY KEY (`id`, `partition_month`(6));
```

#### 基于数据区分度
根据具体的数据区分度来设置前缀索引长度。

假设需要为urls建立索引，可以事先查看当前字段应用不同前缀长度的区分度。
如下是取当前urls前缀长度为1000、100、125、150、200时的数据区分度：
``` sql
select count(distinct urls)/count(*) as urls_full, count(distinct left(urls,100))/count(*) as urls_100, count(distinct left(urls,125))/count(*) as urls_125, count(distinct left(urls,150))/count(*) as urls_150, count(distinct left(urls,200))/count(*) as urls_200 from base_info;
+----------+----------+----------+----------+----------+
| urls_full | urls_100 | urls_125 | urls_150 | urls_200 |
+----------+----------+----------+----------+----------+
|   0.9859 |   0.9858 |   0.9858 |   0.9859 |   0.9859 |
+----------+----------+----------+----------+----------+
```

从结果可以看出，当urls取前缀长度为150时，其区分度与完整长度一致。

<!-- more -->

## 四、知识点

### 1. 前缀索引介绍
对于string类型的字段而言，可以使用字段值的主要部分用作索引。其语法为：`col_name(length)`，用以指定索引前缀长度。
CHAR, VARCHAR, BINARY, and VARBINARY 类型字段可以使用前缀索引。
BLOB and TEXT 类型字段必须使用前缀索引。

前缀索引长度单位为字节(bytes)。然而，CREATE TABLE、ALTER TABLE、CREATE INDEX 语句涉及的字段长度单位有如下两种情况：对于非二进制类型(CHAR, VARCHAR, TEXT)字段而言，这个长度指的是字符数；而对于二进制(BINARY, VARBINARY, BLOB)字段而言，这个长度指的是字节数。

官方说明(详见参考文档2):
> For string columns, indexes can be created that use only the leading part of column values, using col_name(length) syntax to specify an index prefix length:
Prefixes can be specified for CHAR, VARCHAR, BINARY, and VARBINARY column indexes.
Prefixes must be specified for BLOB and TEXT column indexes.
Prefix limits are measured in bytes, whereas the prefix length in CREATE TABLE, ALTER TABLE, and CREATE INDEX statements is interpreted as number of characters for nonbinary string types (CHAR, VARCHAR, TEXT) and number of bytes for binary string types (BINARY, VARBINARY, BLOB). Take this into account when specifying a prefix length for a nonbinary string column that uses a multibyte character set.
For spatial columns, prefix values cannot be given, as described later in this section.

### 2. 字符占用大小计算
字符长度转换成字节数时，需要考虑表字段的字符编码方式。
简单来说，就是字符编码方式不同，1个字符占用的字节数不同。

常用的字符编码方式与对应的占用大小关系如下：

| 字符编码 | 字符占用大小 |
|---|---|
| latin | 单字节：1B |
| GBK | 双字节：2B |
| UTF8 | 三字节：3B |
| UTF8mb4 | 四字节：4B |

### 3. 前缀索引限制
是否支持前缀索引以及前缀索引长度大小，依赖于数据表使用的存储引擎。
对于INNODB存储引擎而言，默认前缀长度最大能支持767字节；而在开启`innodb_large_prefix`属性值的情况下，最大能支持3072字节。
对于MyISAM存储引擎而言，前缀长度限制为1000字节。
对于NDB存储引擎而言，并不支持前缀索引。

官方说明(详见参考文档2):
> Prefix support and lengths of prefixes (where supported) are storage engine dependent. For example, a prefix can be up to 767 bytes long for InnoDB tables or 3072 bytes if the innodb_large_prefix option is enabled. For MyISAM tables, the prefix limit is 1000 bytes. The NDB storage engine does not support prefixes (see Section 18.1.6.6, “Unsupported or Missing Features in MySQL Cluster”).



## 五、参考文档
1. [1071 - Specified key was too long; max key length is 767 bytes](http://stackoverflow.com/a/1814594)
2. [CREATE INDEX Syntax](http://dev.mysql.com/doc/refman/5.6/en/create-index.html)
4. [认识mysql前缀索引](http://hidba.org/?p=216)