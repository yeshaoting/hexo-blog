title: EL access a map value by Integer key
tags:
  - el
  - java
categories:
  - 后端开发
date: 2016-02-28 16:24:00
---
el表达式中，默认数值类型为Long型。

## 一、问题描述

**举例说明**：
servlet定义并返回map数据如下：
``` java
Map<Integer, String> map = new HashMap<Integer, String>();
map.put(1, "One");
map.put(2, "Two");
map.put(3, "Three");
```

在jsp页面，通过map[2]不能获取数据Two。因为数值2被装箱成Long型，而map键类型为Integer。
因此，在前端不能通过，报异常。


## 二、解决方案
将数值转成int型，如下所示：
`${map[(2).intValue()]}`


## 三、参考文档
http://stackoverflow.com/questions/924451/el-access-a-map-value-by-integer-key