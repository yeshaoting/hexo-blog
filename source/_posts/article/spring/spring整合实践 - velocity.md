categories:
  - 后端开发
tags:
  - spring
  - java
  - 实践
  - velocity
title: spring整合实践 - velocity
date: 2015-12-29 13:58:00
---

<img src="/asserts/images/logo/spring.png" class="img-logo img-center" />


此段内容说明如何整合spring与velocity。
此文并不介绍如何使用velocity代替jsp作为页面渲染配置(参考：VelocityAndSpringStepByStep)，仅仅只是输出特定格式的html、xml或文本等内容的整合实践。


## 一、配置spring mvc
首先，web.xml添加spring容器配置:spring-beans.xml, spring-mvc.xml.


## velocity engine实例化
对于spring父容器spring-beans.xml，除开启注解，注入配置文件等基本配置外，向容器添加velocity engine对象定义，注入velocityEngine对象。

具体配置片段如下：
``` xml
  <bean id="velocityEngine" class="org.springframework.ui.velocity.VelocityEngineFactoryBean">
    <property name="resourceLoaderPath" value="/WEB-INF/vm/" />
    <property name="configLocation" value="classpath:velocity.properties" />
  </bean>
```

其中，将velocity模板放置在/martinye-demo-spring-velocity/src/main/webapp/WEB-INF/vm目录，将velocity.properties文件放置在/martinye-demo-spring-velocity/src/main/resources目录。
**注**：vm模板也可以放置到src/main/resources目录下。


<!-- more -->

## 二、velocity配置
velocity.properties配置会覆盖默认配置。
默认配置文件的位置：velocity.jar包路径org.apache.velocity.runtime.defaults下（velocity.properties）。

更详细参数配置可参考：configuring_resource_loaders

根据需要我们自定义的配置文件内容，如下所示：
``` properties

# encoding
input.encoding=UTF-8
output.encoding=UTF-8
contentType=text/html;charset=UTF-8


# autoreload when vm changed
file.resource.loader.cache=false
file.resource.loader.modificationCheckInterval=1
velocimacro.library.autoreload=true


# macro config
# 默认情况，velocimacro.library相对于resourceLoaderPath目录
velocimacro.library=macro.vm
#velocimacro.permissions.allow.inline=true
#velocimacro.permissions.allow.inline.to.replace.global=false
#velocimacro.permissions.allow.inline.local.scope=false

#layout
#tools.view.servlet.layout.directory=/WEB-INF/vm/layout/
#tools.view.servlet.error.template=/WEB-INF/vm/error.vm
#tools.view.servlet.layout.default.template=default.vm


# runtime log
runtime.log=velocity.log
runtime.log.logsystem.class=org.springframework.ui.velocity.CommonsLoggingLogSystem
runtime.log.error.stacktrace=true
runtime.log.warn.stacktrace=true
runtime.log.info.stacktrace=false
runtime.log.invalid.reference=true

```


## 三、简单模板
``` xml
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
  <title>$!title</title>
</head>
<body>
  <h1>标题：$!title</h1>
  <div>
    <p>内容：$!content</p>
    <p>macro函数测试：#test()</p>
  </div>
</body>
</html>
```


## 四、模板渲染
使用VelocityEngineUtils能很方便的merge template。
需要准备一个Map，用于传递变量到VLT中。

**代码样例**：
``` java
    Map<String, Object> model = Maps.newHashMap();
    model.put("title", "Velocity Template");
    model.put("content", "Content in Velocity Template");
    
    String text = VelocityEngineUtils.mergeTemplateIntoString(velocityEngine, "test.vm", Constants.ENCODING, model);
```

## 五、参考文档
1. [VelocityAndSpringStepByStep](http://wiki.apache.org/velocity/VelocityAndSpringStepByStep)
2. [velocity.properties配置说明](http://mj4d.iteye.com/blog/1524050)
3. [springmvc 3.1整合velocity](http://blog.csdn.net/glarystar/article/details/6636574)
4. [configuring_resource_loaders](http://velocity.apache.org/engine/releases/velocity-1.5/developer-guide.html#configuring_resource_loaders)

