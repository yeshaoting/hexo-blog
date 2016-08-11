date: 2016-08-04 11:12:26
---

mysql本地参数调整


## 开启mysql执行日志
``` sql
mysql root@localhost:(none)> show global variables like 'general_log%';
+------------------+---------------------------------------+
| Variable_name    | Value                                 |
|------------------+---------------------------------------|
| general_log      | OFF                                   |
| general_log_file | /usr/local/mysql/data/yerba-buena.log |
+------------------+---------------------------------------+

mysql root@localhost:(none)> set global general_log=ON;
```

于是，mysql执行日志都会输出到：/usr/local/mysql/data/yerba-buena.log



