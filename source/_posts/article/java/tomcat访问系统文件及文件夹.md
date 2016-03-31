title: tomcat访问系统文件及文件夹
tags:
  - tomcat
  - java
categories:
  - 后端开发
date: 2016-02-28 16:24:00
---

<img src="/asserts/images/logo/tomcat.png" class="img-logo img-center" />


## 一、进入tomcat目录
> cd /home/yeshaoting/java/server/tomcat/apache-tomcat-6.0.37


## 二、修改web.xml配置
> vim conf/web.xml


## 三、修改listings参数
找到名为 **default** 的servlet配置。
``` xml
    <servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>true</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
```
修改listings参数值为 **true** 。

listings参数含义说明如下：
``` xml
  <!--   listings            Should directory listings be produced if there -->
  <!--                       is no welcome file in this directory?  [false] -->
  <!--                       WARNING: Listings for directories with many    -->
  <!--                       entries can be slow and may consume            -->
  <!--                       significant proportions of server resources.   -->
```

## 四、参考文档
http://blog.csdn.net/guopengzhang/article/details/5948644
http://xueli.blog.51cto.com/3325186/1585859