tags:
  - velocity
title: 找不到自定义的velocity宏资源文件macro.vm
categories: []
date: 2015-12-29 13:58:00
---

[TOC]

异常描述：VelocityException: Velocimacro : Error using VM library : /WEB-INF/vm/macro.vm
问题原因：路径配置错误。


## 一、项目相关配置

### 1. spring-beans.xml
spring bean velocityEngine
``` xml
  <bean id="velocityEngine" class="org.springframework.ui.velocity.VelocityEngineFactoryBean">
    <property name="resourceLoaderPath" value="/WEB-INF/vm/"/>
    <property name="configLocation" value="classpath:velocity.properties"/>
  </bean>
```

### 2. velocity.properties
宏资源文件配置
``` properties
velocimacro.library=/WEB-INF/vm/macro.vm
```


<!-- more -->


## 二、解析异常原因
首先，默认velocity模板加载方式为file，对应的资源加载类为：`org.apache.velocity.runtime.resource.loader.FileResourceLoader`。参见如下 **模板加载配置** 以及 **FileResourceLoader说明** 。

从模板目录/WEB-INF/vm/加载模块文件(`ResourceManagerImpl.getResource`)。

如果模板文件没有缓存(`ResourceManagerImpl.globalCacheglobalCache`)，则从目录文件中加载(`ResourceManagerImpl.loadResource`)。

加载过程中，加载资源文件流，解析并初始化。`org.apache.velocity.Template.process()`
此时，调用资源文件加载类FileResourceLoader加载文件。

FileResourceLoader在file.resource.loader.path目录，即spring-beans.xml中配置的resourceLoaderPath目录通过模板名查找并加载模板。
如果resourceLoaderPath未配置或者为空的话，则通过绝对路径加载模板；否则，相对于resourceLoaderPath查找模板。
如果resourceLoaderPath不为空，模板路径velocimacro.library以/开头，则直接去除/。

### 模板加载配置
默认的velocity.properties in package `org.apache.velocity.runtime.defaults`。
``` properties
resource.loader = file

file.resource.loader.description = Velocity File Resource Loader
file.resource.loader.class = org.apache.velocity.runtime.resource.loader.FileResourceLoader
file.resource.loader.path = .
file.resource.loader.cache = false
file.resource.loader.modificationCheckInterval = 2
```

### FileResourceLoader说明
A loader for templates stored on the file system. Treats the template as relative to the configured root path. If the root path is empty treats the template name as an absolute path.


## 三、小结
因此，对于当前的项目配置来说，velocity框架实际上是在/WEB-INF/vm/WEB-INF/vm/macro.vm目录加载模板文件macro.vm。

**注**：如果希望在class目录加载模板文件，可以考试设置资源文件加载类为：org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader。

修改velocity.properties配置：
``` properties
resource.loader = class
class.resource.loader.class = org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader

velocimacro.library=macro.vm
```

## 四、参考文档
http://stackoverflow.com/questions/3443819/velocity-cant-find-resource
http://velocity.apache.org/engine/releases/velocity-1.6.4/developer-guide.html#Configuring_Resource_Loaders
http://velocity.apache.org/engine/releases/velocity-1.6.4/apidocs/org/apache/velocity/runtime/resource/loader/ClasspathResourceLoader.html