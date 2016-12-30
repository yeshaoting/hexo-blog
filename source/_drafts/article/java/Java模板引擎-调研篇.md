title: Java模板引擎-调研篇
date: 2016-12-30 10:48:00
---

# Java模板引擎-调研篇



## 一、概述





## 一、功能特性对比

![Comparison of web template engines](http://ocszqgifn.bkt.clouddn.com/meituan/Comparison_of_web_template_engines.png)



![](http://ocszqgifn.bkt.clouddn.com/meituan/Snip20160905_21.png)



| 引擎                                       | 变量   | 函数   | 包含   | 条件   | 循环   | 重解析                                      | 赋值   | 异常   | i18n | Natural | 继承   |
| ---------------------------------------- | ---- | ---- | ---- | ---- | ---- | ---------------------------------------- | ---- | ---- | ---- | ------- | ---- |
| [Apache Velocity](https://en.wikipedia.org/wiki/Apache_Velocity) | Yes  | Yes  | Yes  | Yes  | Yes  | Yes                                      | Yes  | Yes  |      |         | No   |
| [action4JAVA](http://www.action4java.org/) | Yes  | No   | Yes  | Yes  | Yes  | No                                       | Yes  | Yes  |      |         |      |
| [Chunk Templates](http://www.x5software.com/chunk/) | Yes  | Yes  | Yes  | Yes  | Yes  | No                                       | Yes  | Yes  | Yes  | Yes     | Yes  |
| [CodeCharge Studio](https://en.wikipedia.org/wiki/CodeCharge_Studio) | Yes  | Yes  | Yes  | Yes  | Yes  | Yes                                      | Yes  | No   | Yes  | Yes     |      |
| [FreeMarker](https://en.wikipedia.org/wiki/FreeMarker) | Yes  | Yes  | Yes  | Yes  | Yes  | Yes                                      | Yes  | Yes  | Yes  | No      | No   |
| [Hamlets](https://en.wikipedia.org/wiki/Hamlets) | Yes  | Yes  | Yes  | Yes  | Yes  | No                                       | Yes  | Yes  |      |         |      |
| [HTTL](http://httl.github.io/en/)        | Yes  | Yes  | Yes  | Yes  | Yes  | Yes                                      | Yes  |      |      | Yes     |      |
| [JavaServer Pages](https://en.wikipedia.org/wiki/JavaServer_Pages) | Yes  | Yes  | Yes  | Yes  | Yes  | Yes                                      | Yes  | Yes  |      |         |      |
| [jin-template](https://code.google.com/p/jin-template/) | Yes  | No   | No   | No   | No   | No                                       | Yes  | No   |      |         |      |
| [MiniTemplator](http://www.source-code.biz/MiniTemplator/) | Yes  | Yes  | Yes  | Yes  | Yes  | No                                       | No   | No?  |      |         |      |
| [Pebble](http://mitchellbosecke.com/pebble/) | Yes  | Yes  | Yes  | Yes  | Yes  | Yes                                      | Yes  | Yes  | Yes  | No      | Yes  |
| [Rythm](http://rythmengine.org/)         | Yes  | Yes  | Yes  | Yes  | Yes  | Yes ([Java](https://en.wikipedia.org/wiki/Java_(programming_language))) | Yes  | Yes  | Yes  | Yes     | Yes  |
| [Scalate](http://scalate.fusesource.org/) | Yes  | Yes  | Yes  | Yes  | Yes  | Yes ([Scala](https://en.wikipedia.org/wiki/Scala_(programming_language))) | Yes  | Yes  |      |         |      |
| [StringTemplate](http://www.stringtemplate.org/) | Yes  | No   | Yes  | Yes  | Yes  | No                                       | No   | No   |      |         | No   |
| [Thymeleaf](https://en.wikipedia.org/wiki/Thymeleaf) | Yes  | Yes  | Yes  | Yes  | Yes  | Yes                                      | Yes  | Yes  | Yes  | Yes     | No   |
| [WebMacro](https://en.wikipedia.org/wiki/WebMacro) | Yes  | Yes  | Yes  | Yes  | Yes  | Yes                                      | Yes  | Yes  |      |         |      |



Spring支持

时间



## 二、渲染性能对比







## 参考文档

1. [Comparison of web template engines](https://en.wikipedia.org/wiki/Comparison_of_web_template_engines)







http://wiki.sankuai.com/display/~zhangyancai/Velocity

http://wiki.sankuai.com/pages/viewpage.action?pageId=87571056

http://wiki.sankuai.com/pages/viewpage.action?pageId=475095159



http://velocity.apache.org/engine/devel/vtl-reference.html

http://velocity.apache.org/engine/devel/user-guide.html#evaluate

http://httl.github.io/zh/syntax.html#JSON函数

http://httl.github.io/zh/syntax.html#Velocity对比



https://github.com/jreijn/spring-comparing-template-engines

http://stackoverflow.com/questions/1216267/ab-program-freezes-after-lots-of-requests-why/1217100#1217100

http://web.archive.org/web/20090210151520/

http://www.brianp.net/2008/10/03/changing-the-length-of-the-time_wait-state-on-mac-os-x/

https://github.com/hayde/templateEngine

https://github.com/hayde/templateGUI/tree/master/nbproject



https://java-source.net/open-source/template-engines

http://www.cnblogs.com/firstyi/archive/2007/11/01/945745.html

http://www.slideshare.net/jreijn/comparing-templateenginesjvm#!

http://www.javaworld.com/article/2077797/open-source-tools/velocity-or-freemarker.html?page=3

http://www.iteye.com/topic/291280

http://www.iteye.com/topic/1114669

https://www.zhihu.com/question/28466557



https://en.wikipedia.org/wiki/Comparison_of_web_template_engines

https://en.wikipedia.org/wiki/Category:Template_engines

https://en.wikipedia.org/wiki/Web_template_system

https://en.wikipedia.org/wiki/Template_processor



http://stackoverflow.com/questions/2381619/best-template-engine-in-java