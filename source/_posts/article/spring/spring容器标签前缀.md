tags:
  - spring
  - issue
categories:
  - java
title: spring容器标签前缀
date: 2016-02-28 13:53:00
---

一般地，spring容器标签bean元数据格式为`<bean></bean>`。但是，奇怪的是有的时候必须使用`<beans:bean></beans:bean>`，否则会报错。

## 一、为什么会这样？
这与spring容器xml配置当前主要的命名空间(xmlns)设置有关。
由于在同一个xml配置文件中可能会加载多个文档结构定义(DTD)，因此需要使用命名空间来区分开多个DTD中可能相同的文档标签。

例如：spring **tx** schema包含annotation-driven(`<tx:annotation-driven/>`)，同时，spring mvc schema也包含annotation-driven(`<mvc:annotation-driven/>`)

## 二、小结
对于以上问题描述情况，不需要使用beans前缀的，xmlns是如下情况：
``` xml
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:beans="http://www.springframework.org/schema/beans">
</beans>
```

而对于需要使用beans前缀的，xmlns被修改了，可能是：
``` xml
<beans xmlns="http://www.springframework.org/schema/mvc"
	   xmlns:beans="http://www.springframework.org/schema/beans">
</beans>
```

**由此可知**：
> 使用主命名空间里的标签，命名空间前缀并不是必须的。

## 三、参考文档
http://bbs.csdn.net/topics/340026084
http://www.w3school.com.cn/tags/tag_prop_xmlns.asp