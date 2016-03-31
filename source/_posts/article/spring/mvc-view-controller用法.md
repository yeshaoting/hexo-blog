title: mvc-view-controller用法
tags:
  - spring
  - java
categories:
  - 后端开发
date: 2016-02-28 13:57:00
---

<img src="/asserts/images/spring.png" class="img-logo img-center" />

## 一、重定向
``` xml
<mvc:view-controller path="/" view-name="redirect:/admin/index"/>
```
说明：即如果当前路径是/，则重定向到/admin/index


## 二、view name
```
<mvc:view-controller path="/" view-name="admin/index"/>
```
如果当前路径是/，则交给相应的视图解析器直接解析为视图
如：
``` xml
<bean id="defaultViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver" p:order="2">
	<property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
	<property name="contentType" value="text/html"/>
	<property name="prefix" value="/WEB-INF/jsp/"/>
	<property name="suffix" value=".jsp"/>
</bean>
```
则得到的视图是：/WEB-INF/jsp/admin/index.jsp


## 三、参考
http://www.iteye.com/topic/1129445