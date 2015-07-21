
categories:
 - 数据库

tags:
 - mysql
 - 小记

title: mysql数据库命令小记
---

[TOC]

## 查看锁情况
表锁：show global status like 'table_locks%';
行锁：show global status like 'innodb_row_lock%';


## 文件打开数
show global status like 'open_files';


## 查看慢查询
show full processlist;
SELECT * FROM information_schema.INNODB_TRX;
select @@autocommit;


## 设置对应的编码
set character_set_server=utf8;


## 查看数据库编码
SHOW VARIABLES LIKE 'character%';

<!-- more -->

## 查看日期毫秒数
select unix_timestamp('2009-10-26 10:06:07');


## 查看日期
select from_unixtime(1430211829676/1000);


## 查看表索引
show index from table;


## 查看数据库表字段
select COLUMN_NAME from information_schema.COLUMNS
where table_name = 'channel_movie' 
and table_schema = 'movie_ticket';


## 数据库事务隔离级别

### 查看当前会话隔离级别
select @@tx_isolation;

### 查看系统当前隔离级别
select @@global.tx_isolation;

### 设置当前会话隔离级别
set session transaction isolatin level repeatable read;

### 设置系统当前隔离级别
set global transaction isolation level repeatable read;


## 显示数据表索引
show index from user_login_status;


## 删除重复数据（性能不佳）
``` sql
set sql_safe_updates=0;
delete from netnews.item
where id not in (
     select id from (
          select  min(id) as id from netnews.item group by channel_id, link
     ) as tab
);
set sql_safe_updates=1;
```


## 安全更新状态修改
关闭：set sql_safe_updates=0;
打开：set sql_safe_updates=1;


## 查看mysql系统状态
show status
show processlist


## 临时修改内容显示字符集
charset utf8


## 修改索引
ALTER TABLE `babel_idea`.`i_product_owner`
DROP INDEX `name_UNIQUE` ,
ADD UNIQUE INDEX `unique_product_owner_name` (`name` ASC);


## 显示索引
show index from i_product;


## 删除索引
alter table i_product drop index fk_product_deleted_idx;


## 按条件导出mysql表数据
`mysqldump -u root -p 数据库名 --no-create-db=TRUE --no-create-info=TRUE --add-drop-table=FALSE --where="id >1000" 表明>导出文件名.sql`


## mysqldump数据导入
mysqldump babel_idea < babel_idea.sql


## 启用safe mode(update, delete, select操作需要包含主键的where条件)
set sql_safe_updates=1;


## 关闭safe mode
set sql_safe_updates=0;


## 修改表字段
alter table i_product
add column `deleted` tinyint not null
default 0
comment '逻辑删除，是否已删除(0: 未删除，1: 删除)';



## 添加索引

### 添加单一索引
alter table i_product
drop index `fk_product_product_idx`;

### 添加联合索引
alter table i_product
add index `fk_product_id_parent_idx`
(id, parent);


## 创建数据库(设置默认字符集)
create database `babel_idea` DEFAULT CHARACTER SET utf8;


## 只导出数据库数据表结构
~/local/mysql/bin/mysqldump -ubkmadmin -pbkmadmin -t babel_idea > ~/babel_idea.schema.sql

