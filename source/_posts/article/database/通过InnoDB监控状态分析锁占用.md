

## 一、表结构
``` sql
CREATE TABLE `t` (
  `id` int(11) NOT NULL,
  `stage` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `idx_b` (`stage`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```


## 二、表数据
``` bash
+------+---------+
|   id |   stage |
|------+---------|
|    1 |       1 |
|    4 |       4 |
|    9 |       9 |
|   15 |      15 |
+------+---------+
```


## 三、简单实例

### 1. 事务执行时序表
事务(ID:41114)，在索引idx_b值为4上加排他锁。


| T1(41114) |
|---|
| begin; |
| select * from t where stage = 4 for update; |

### 2. 查看InnoDB运行状态
``` bash
---TRANSACTION 41114, ACTIVE 41 sec
4 lock struct(s), heap size 1184, 3 row lock(s)
MySQL thread id 39, OS thread handle 0x700000a83000, query id 36756 localhost ::1 root cleaning up
TABLE LOCK table `test`.`t` trx id 41114 lock mode IX
RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41114 lock_mode X
Record lock, heap no 7 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 80000004; asc     ;;
 1: len 4; hex 80000004; asc     ;;

RECORD LOCKS space id 245 page no 3 n bits 80 index `PRIMARY` of table `test`.`t` trx id 41114 lock_mode X locks rec but not gap
Record lock, heap no 6 PHYSICAL RECORD: n_fields 4; compact format; info bits 0
 0: len 4; hex 80000004; asc     ;;
 1: len 6; hex 00000000a08b; asc       ;;
 2: len 7; hex dd000001fa0110; asc        ;;
 3: len 4; hex 80000004; asc     ;;

RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41114 lock_mode X locks gap before rec
Record lock, heap no 8 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 80000009; asc     ;;
 1: len 4; hex 80000009; asc     ;;
```

### 3. 解读InnoDB运行状态
事务T1总共加了4把锁：1把表，3把行锁。
表锁类型：IX

行锁根据索引划分为加在聚集索引 PRIMARY 上的行锁和加在普通索引 idx_b 上的行锁。
加在聚集索引上的行锁1把，是一个排他的记录锁；
加在普通索引上的行锁2把，1把是next-key lock，锁住的范围为(1, 4]；1把是gap lock，锁住的范围为(4, 9)。


## 四、复杂实例
下面演示一个更加复杂的实例：GAP与Insert Intention冲突引发的死锁，分析死锁时各事务锁占用情况。

### 1. 事务执行时序表
| T1(41121) | T1(41122) |
|---|---|
| begin; | begin; |
| select * from t where stage = 4 for update; | |
| | select * from t where stage = 9 for update; |
| insert INTO t values (6, 6) | |
| | insert INTO t values (7, 7) |
| | (1213, u'Deadlock found when trying to get lock; try restarting transaction') |
| Query OK, 1 row affected | &nbsp; |

事务T1首先锁住stage=4的记录，然后插入(6, 6)的记录；
事务T2首先锁住stage=9的记录，然后插入(7, 7)的记录。


### 2. 查看InnoDB运行状态
由于发生死锁后，其中一个事务会回滚，而无法查看到其事务锁占用状态。
因此，我们在在事务T2执行记录插入操作之前，查看一下当前各事务锁占用状态。如下所示：

``` bash
------------
TRANSACTIONS
------------
Trx id counter 41123
Purge done for trx's n:o < 41120 undo n:o < 0 state: running but idle
History list length 968
LIST OF TRANSACTIONS FOR EACH SESSION:
---TRANSACTION 0, not started
MySQL thread id 67, OS thread handle 0x700000ac7000, query id 37547 localhost ::1 root init
show ENGINE innodb status
---TRANSACTION 41105, not started
MySQL thread id 65, OS thread handle 0x700000b0b000, query id 36363 localhost 127.0.0.1 root cleaning up
---TRANSACTION 0, not started
MySQL thread id 64, OS thread handle 0x700000b4f000, query id 36362 localhost 127.0.0.1 root cleaning up
---TRANSACTION 0, not started
MySQL thread id 58, OS thread handle 0x700000a3f000, query id 37546 localhost root cleaning up
---TRANSACTION 41122, ACTIVE 26 sec
4 lock struct(s), heap size 1184, 3 row lock(s)
MySQL thread id 69, OS thread handle 0x700000bd7000, query id 37534 localhost ::1 root cleaning up
TABLE LOCK table `test`.`t` trx id 41122 lock mode IX
RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41122 lock_mode X
Record lock, heap no 8 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 80000009; asc     ;;
 1: len 4; hex 80000009; asc     ;;

RECORD LOCKS space id 245 page no 3 n bits 80 index `PRIMARY` of table `test`.`t` trx id 41122 lock_mode X locks rec but not gap
Record lock, heap no 7 PHYSICAL RECORD: n_fields 4; compact format; info bits 0
 0: len 4; hex 80000009; asc     ;;
 1: len 6; hex 00000000a08b; asc       ;;
 2: len 7; hex dd000001fa011d; asc        ;;
 3: len 4; hex 80000009; asc     ;;

RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41122 lock_mode X locks gap before rec
Record lock, heap no 9 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 8000000f; asc     ;;
 1: len 4; hex 8000000f; asc     ;;

---TRANSACTION 41121, ACTIVE 34 sec inserting
mysql tables in use 1, locked 1
LOCK WAIT 5 lock struct(s), heap size 1184, 4 row lock(s), undo log entries 1
MySQL thread id 39, OS thread handle 0x700000a83000, query id 37538 localhost ::1 root update
insert INTO t values (6, 6)
------- TRX HAS BEEN WAITING 21 SEC FOR THIS LOCK TO BE GRANTED:
RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41121 lock_mode X locks gap before rec insert intention waiting
Record lock, heap no 8 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 80000009; asc     ;;
 1: len 4; hex 80000009; asc     ;;

------------------
TABLE LOCK table `test`.`t` trx id 41121 lock mode IX
RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41121 lock_mode X
Record lock, heap no 7 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 80000004; asc     ;;
 1: len 4; hex 80000004; asc     ;;

RECORD LOCKS space id 245 page no 3 n bits 80 index `PRIMARY` of table `test`.`t` trx id 41121 lock_mode X locks rec but not gap
Record lock, heap no 6 PHYSICAL RECORD: n_fields 4; compact format; info bits 0
 0: len 4; hex 80000004; asc     ;;
 1: len 6; hex 00000000a08b; asc       ;;
 2: len 7; hex dd000001fa0110; asc        ;;
 3: len 4; hex 80000004; asc     ;;

RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41121 lock_mode X locks gap before rec
Record lock, heap no 8 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 80000009; asc     ;;
 1: len 4; hex 80000009; asc     ;;

RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41121 lock_mode X locks gap before rec insert intention waiting
Record lock, heap no 8 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 80000009; asc     ;;
 1: len 4; hex 80000009; asc     ;;

```

### 3. 死锁日志
事务T2执行记录插入操作，发生死锁。
死锁日志如下所示：

``` bash
------------------------
LATEST DETECTED DEADLOCK
------------------------
2016-08-04 13:39:05 700000bd7000
*** (1) TRANSACTION:
TRANSACTION 41121, ACTIVE 54 sec inserting
mysql tables in use 1, locked 1
LOCK WAIT 5 lock struct(s), heap size 1184, 4 row lock(s), undo log entries 1
MySQL thread id 39, OS thread handle 0x700000a83000, query id 37538 localhost ::1 root update
insert INTO t values (6, 6)
*** (1) WAITING FOR THIS LOCK TO BE GRANTED:
RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41121 lock_mode X locks gap before rec insert intention waiting
Record lock, heap no 8 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 80000009; asc     ;;
 1: len 4; hex 80000009; asc     ;;

*** (2) TRANSACTION:
TRANSACTION 41122, ACTIVE 46 sec inserting
mysql tables in use 1, locked 1
5 lock struct(s), heap size 1184, 4 row lock(s), undo log entries 1
MySQL thread id 69, OS thread handle 0x700000bd7000, query id 37555 localhost ::1 root update
insert INTO t values (7, 7)
*** (2) HOLDS THE LOCK(S):
RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41122 lock_mode X
Record lock, heap no 8 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 80000009; asc     ;;
 1: len 4; hex 80000009; asc     ;;

*** (2) WAITING FOR THIS LOCK TO BE GRANTED:
RECORD LOCKS space id 245 page no 4 n bits 80 index `idx_b` of table `test`.`t` trx id 41122 lock_mode X locks gap before rec insert intention waiting
Record lock, heap no 8 PHYSICAL RECORD: n_fields 2; compact format; info bits 0
 0: len 4; hex 80000009; asc     ;;
 1: len 4; hex 80000009; asc     ;;

*** WE ROLL BACK TRANSACTION (2)
```

由上述死锁日志可知：两个事务都在等待加在这个位置的锁 **space id 245 page no 4 n bits 80**。
另外，


### 4. 解读InnoDB运行状态

#### 事务T1
事务T1总共 **要加** 5把锁：1把表，4把行锁。

**表锁**
表锁类型：IX

**行锁**
idx_b 上的排他 next-key lock，范围：idx_b in (1, 4]。
内存位置：space id 245 page no 4 n bits 80，heap no 7 PHYSICAL RECORD: n_fields 2;

primary 上的排他 record lock，范围：primary=4。
内存位置：space id 245 page no 3 n bits 80，heap no 6 PHYSICAL RECORD: n_fields 4;

idx_b 上的排他 gap lock，范围：idx_b in (4, 9)。
内存位置：space id 245 page no 4 n bits 80，heap no 8 PHYSICAL RECORD: n_fields 2;

idx_b 上的排他 insert intention lock，范围：idx_b in (4, 9)。
内存位置：space id 245 page no 4 n bits 80，heap no 8 PHYSICAL RECORD: n_fields 2;

其中，insert intention lock 是正等待加的锁。


#### 事务T2
**注**：当前分析事务T2占用的锁快照为T2执行插入语句之前状态。

事务T2总共加了4把锁：1把表，3把行锁。

**表锁**
表锁类型：IX

**行锁**
idx_b 上的排他 next-key lock，范围：idx_b in (4， 9]
内存位置：space id 245 page no 4 n bits 80，heap no 8 PHYSICAL RECORD: n_fields 2;

primary 上的排他 record lock，范围：primary=9
内存位置：space id 245 page no 3 n bits 80，heap no 7 PHYSICAL RECORD: n_fields 4;

idx_b 上的排他 gap lock，范围：idx_b in (9， 15)
内存位置：space id 245 page no 4 n bits 80，heap no 9 PHYSICAL RECORD: n_fields 2;


#### 死锁分析
在事务T1插入记录前，事务T1拿到了范围idx_b in (4, 9)的gap lock，事务T2拿到了范围idx_b in (4， 9]上的next-key lock。

事务T1插入记录(6, 6)时，要请求范围idx_b in (4, 9)上的insert intention lock，而请求的insert intention lock与事务T2已持有next key lock是互斥的。
于是，事务T1 block，等待事务T2释放范围idx_b in (4， 9]上的next-key lock。

事务T2插入记录(7, 7)时，要请求范围idx_b in (4, 9)上的insert intention lock，而请求的insert intention lock与事务T1已持有的gap lock是互斥的。
于是，事务T2也会block，等待事务T2释放范围idx_b in (4， 9)上的gap lock。
死锁便产生了。


