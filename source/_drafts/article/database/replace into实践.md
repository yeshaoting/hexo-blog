date: 2016-02-15 15:27:08
title: replace into实践
---

## 用法简介
查阅mysql官方[参考文档](http://dev.mysql.com/doc/refman/5.7/en/replace.html)，有如下二段内容：
> REPLACE works exactly like INSERT, except that if an old row in the table has the same value as a new row for a PRIMARY KEY or a UNIQUE index, the old row is deleted before the new row is inserted. See Section 13.2.5, “INSERT Syntax”.
大致意思是：replace用法与insert比较像，只是如果表中的一个旧行具有相同的值作为一个主键或唯一索引的新行，插入新行之前旧的行被删除。



> REPLACE is a MySQL extension to the SQL standard. It either inserts, or deletes and inserts. For another MySQL extension to standard SQL—that either inserts or updates—see Section 13.2.5.3, “INSERT ... ON DUPLICATE KEY UPDATE Syntax”.


## 参考文档
1. [Mysql中replace into用法详细说明](http://my.oschina.net/junn/blog/110213?fromerr=8w7ArO5a)
2. [语法：MySQL中INSERT IGNORE INTO和REPLACE INTO的使用](http://www.cnblogs.com/RoadGY/archive/2011/09/28/2193733.html)
3. [sql replace into 用法与实现语句](http://www.111cn.net/database/mysql/41017.htm)


