title: mysql LATEST DETECTED DEADLOCK
tags:
  - mysql
categories:
  - mysql
date: 2015-12-29 20:46:00
---

# mysql LATEST DETECTED DEADLOCK

``` sql
------------------------
LATEST DETECTED DEADLOCK
------------------------
130819 14:01:12
*** (1) TRANSACTION:
TRANSACTION 0 108626388, ACTIVE 0 sec, process no 8726, OS thread id 47220783470912
starting index read
mysql tables in use 1, locked 1
LOCK WAIT 4 lock struct(s), heap size 1216, 2 row lock(s), undo log entries 1
MySQL thread id 4283, query id 21974219 10.92.210.108 ddrsrbe Updating
update ddrsproduction.requests set request_priority_grade_id=3, costs=0, data_version=1, date_completed='2013-08-19 13:59:39', date_end='2013-05-24 23:59:00', date_required_by='2013-08-26 13:57:31', date_start='2013-05-19 00:01:00', date_submitted='2013-08-19 13:57:31', designated_authority_id=84528, is_manual=1, missed_sla_reason=null, missed_sla_reason_ask_user=null, product_id=35, request_cost_status_id=0, request_delivery_method_id=0, request_legislation_id=0, request_method_id=0, request_reason_id=1, request_result_status_id=null, origin_id=0, request_status_id=2, request_type_id=1, result_row_count=2, results_last_downloaded=null, site_processed='KNOW', time_zone=null, urn='LBP/281/35/12 (CONS)', user_id=2357, vf_rep_id=8 where request_id=132536
*** (1) WAITING FOR THIS LOCK TO BE GRANTED:
```

参见：
[MySQL Innodb Deadlock - how to remove old event?](http://stackoverflow.com/questions/21573684/mysql-innodb-deadlock-how-to-remove-old-event)
[How can I stop a running MySQL query?](http://stackoverflow.com/questions/3787651/how-can-i-stop-a-running-mysql-query)
[mysql thread入门分析](http://blog.csdn.net/wyzxg/article/details/8258033)
[MySQL查询阻塞语句](http://blog.itpub.net/29254281/viewspace-1420158/)
[如何 查找 mysql 中如何 kill 引起死锁的线程id](http://zhidao.baidu.com/link?url=Dbx5nrJ-HniRjqzX4UvKkXdoIAhUwCsMfriWuVBZFPTtNfkTWxQYtFz6WFNM54STI3AhdNkzP7QduoJNKTlPIfSHsLtv6Ql63lyDKmq9qT3)
[Trying to understand MySQL deadlock on InnoDB table](http://stackoverflow.com/questions/11331029/trying-to-understand-mysql-deadlock-on-innodb-table)
