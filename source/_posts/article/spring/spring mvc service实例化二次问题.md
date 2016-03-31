categories:
  - 后端开发
tags:
  - spring
  - java
  - mvc
  - issue
title: spring mvc service实例化二次问题
date: 2015-12-29 13:58:00
---

<img src="/asserts/images/spring.png" class="img-logo img-center" />

描述：错误地spring容器配置，导致spring实例service二次。


## 项目现状

### web.xml配置
web容器，首先加载spring-beans.xml配置，再加载spring-mvc.xml配置。
``` xml
  <!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-beans.xml</param-value>
  </context-param>

  <!-- Creates the Spring Container shared by all Servlets and Filters -->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <!-- Processes application requests -->
  <servlet>
    <servlet-name>spring</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>spring</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
```


<!-- more -->


### root spring配置
**期望**通过注解实例化除Controller之外的对象。
``` xml
  <!-- 使用annotation 自动注册bean, 并保证@Required、@Autowired的属性被注入 -->
  <context:component-scan base-package="com.martinye.demo">
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    <context:exclude-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice" />
  </context:component-scan>
```

### mvc spring配置
**期望**通过注解实例化只有Controller的对象。
``` xml
  <!-- 使用annotation 自动注册bean, 并保证@Required、@Autowired的属性被注入 -->
  <context:component-scan base-package="com.martinye.demo">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    <context:include-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice" />
  </context:component-scan>
```


## 问题小析
通过如此配置，发现spring在加载spring-beans.xml时，实例化了一次service，再加载spring-mvc.xml时，实例化了一次controller和一次service，导致spring先后总共实例化二次service的情况发生。

### 解析方案1
spring-mvc.xml扫描时，base-package设置更细粒度的子包。如：
``` xml
  <!-- 使用annotation 自动注册bean, 并保证@Required、@Autowired的属性被注入 -->
  <context:component-scan base-package="com.martinye.demo.controller">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    <context:include-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice" />
  </context:component-scan>
```

此配置的含义：只扫描com.martinye.demo.controller包路径下子标签指明的类型，以及**默认的**@Component、@ManagedBean、@Named注解类型。

**注**：子标签与注解类型冲突，以子标签配置为准。


### 解析方案2
设置use-default-filters值为false，其值默认为true。如：
``` xml
  <!-- 使用annotation 自动注册bean, 并保证@Required、@Autowired的属性被注入 -->
  <context:component-scan base-package="com.martinye.demo" use-default-filters＝"false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    <context:include-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice" />
  </context:component-scan>
```

此配置的含义：只扫描com.martinye.demo包路径下子标签指明的类型，不自动注册**默认的**@Component、@ManagedBean、@Named注解类型。

**注**：子标签与注解类型冲突，以子标签配置为准。


### 结语
查阅了相关文档，发现：
1. spring`<context:component-scan>`标签处理逻辑会交给org.springframework.context.config.ContextNamespaceHandler处理，然后季托ComponentScanBeanDefinitionParser会读取配置文件信息并组装成org.springframework.context.annotation.ClassPathBeanDefinitionScanner进行处理。
2. context:component-scan的子标签，作用有先后顺序，越上面的子标签优先越高。
3. use-default-filters为true时，实例化ClassPathBeanDefinitionScanner时，会调用registerDefaultFilters，include @Component、@ManagedBean、@Named注解类；否则不include。之后再添加include-filter和exclude-filter里的逻辑。



## 参考文档
- [context:component-scan扫描使用上的容易忽略的use-default-filters](http://jinnianshilongnian.iteye.com/blog/1762632)
- [spring配置中<context:component-scan/>的use-default-filters的作用](http://liuluo129.iteye.com/blog/1943412)


