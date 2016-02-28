title: el数值运算取整
tags:
  - el
categories:
  - java
date: 2016-02-28 16:24:00
---

## 取整
el表达式除法结果为浮点型，无法通过运算直接得到整数值。
通过formatNumber保留整除后0位有效小数即可。
如下所示：
``` xml
<fmt:formatNumber type="number" value="${8/7)}" maxFractionDigits="0"/>
```

## 参考文档
http://blog.csdn.net/debbykindom/article/details/5854750