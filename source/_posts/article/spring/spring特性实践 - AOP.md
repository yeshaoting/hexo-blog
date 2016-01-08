categories:
  - spring
tags:
  - spring
  - spring特性实践
title: spring特性实践 - AOP
date: 2015-12-29 13:58:00
---

此段内容说明如何使用spring aop，并通过一个函数耗时统计切面demo来讲述使用具体步骤。

[TOC]


## 一、创建maven工程

通过STS创建一个maven工程martinye-demo-spring-support。

修改pom.xml文件，添加项目相关依赖如下：

**servlet**
- javax.servlet-api
- jstl
- standard
- jsp-api

**spring**
- spring-webmvc
- aspectjweaver

**log**
- slf4j-api
- slf4j-log4j12

**utils**
- commons-lang3
- commons-collections
- commons-io
- guava
- fastjson

**test**
- junit
- spring-test


<!-- more -->

## 二、配置servlet容器
配置路径：/martinye-demo-spring-velocity/src/main/webapp/WEB-INF/web.xml

web servlet容器web.xml，添加加载spring容器的配置。
根据关注点不同，我们把spring容器配置定义分开，如下所示：

### 定义spring root容器
实例化类对象。

配置如下：
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
```

### 定义spring mvc容器
处理用户请求。
spring mvc容器对应的ApplicationContext是spring root容器对应的ApplicationContext的子类对象，可持有spring root容器中注入的类对象。

配置如下：
``` xml
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


## 三、配置spring-beans.xml
配置如下：
``` xml
  <context:property-placeholder ignore-resource-not-found="true" location="classpath*:app.properties" />

  <!-- 使用annotation 自动注册bean, 并保证@Required、@Autowired的属性被注入 -->
  <context:component-scan base-package="com.martinye.demo" use-default-filters="true">
    <context:include-filter type="annotation" expression="org.aspectj.lang.annotation.Aspect"/>
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    <context:exclude-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice" />
  </context:component-scan>

  <aop:aspectj-autoproxy expose-proxy="true" proxy-target-class="false"/>
```

**配置描述**：

1. 加载配置资源：app.properties。

2. 启用注解功能
除Controller以外的Component注解注释的类，自动注入@Required, @Autowired, @PostConstruct, @PreDestroy, @Resource, @PersistenceContext and @PersistenceUnit注释的属性。

3. 开启aop，启用@AspectJ注解。
在注入当前容器的类对象方法上，支持AOP注解。


## 四、配置spring-mvc.xml
配置如下：
``` xml
  <context:property-placeholder ignore-resource-not-found="true" location="classpath*:app.properties" />

  <!-- 使用annotation 自动注册bean, 并保证@Required、@Autowired的属性被注入 -->
  <context:component-scan base-package="com.martinye.demo" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.aspectj.lang.annotation.Aspect"/>
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    <context:include-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice" />
  </context:component-scan>

  <!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
  <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/views/" />
    <property name="suffix" value=".jsp" />
  </bean>

  <aop:aspectj-autoproxy expose-proxy="true" proxy-target-class="false"/>
```

**配置描述**：

1. 加载配置资源：app.properties。

2. 启用注解功能
禁用默认的包扫描过滤策略，只扫描Controller注解。

3. 开启aop，启用@AspectJ注解。
在注入当前容器的类对象方法上，支持AOP注解。


## 五、创建TimeCost注解
通过注解来开关是否需要进行方法耗时统计。

``` java
package com.martinye.demo.support.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * @description
 * 
 * @author yeshaoting
 * @date 2015-05-07 13:45:29
 */
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface TimeCost {


}

```


## 六、创建TimeCost切面

切面检测加有TimeCost的方法。

为了保证统计完整性，防止多个切面逻辑耗时未计算的问题，我们为切面添加切面优先级。
此优先级通过在切面类上添加`@Order`注解来完成。

``` java
package com.martinye.demo.support.aspect;

import org.apache.commons.lang3.time.StopWatch;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.core.annotation.Order;

/**
 * @description 方法耗时切面
 * 
 * @author yeshaoting
 * @date 2015-06-11 11:18:06
 */
@Aspect
@Order(8)
public class TimeCostAspect {
  
  private Logger logger = LoggerFactory.getLogger(this.getClass());
  
  @Pointcut("@annotation(com.martinye.demo.support.annotation.TimeCost)")
  public void point() {};
  
  @Before(value = "point()")
  public void beforeAdvice(JoinPoint joinPoint) {
    logger.info("======= before advice {} ======", joinPoint.getSignature().getName());
  }
  
  @After(value = "point()")
  public void afterAdvice(JoinPoint joinPoint) {
    logger.info("======= after advice {} ======", joinPoint.getSignature().getName());
  }
  
  @Around(value = "point()")
  public void aroundAdvice(ProceedingJoinPoint joinPoint) throws Throwable {
    logger.info("======= before around advice {} ======", joinPoint.getSignature().getName());
    
    StopWatch clock = new StopWatch();
    clock.start(); // 计时开始
    
    try {
      joinPoint.proceed();
    } catch (Exception e) {
      throw e;
    } finally {
      clock.stop(); // 计时结束
      long timeCost = clock.getTime();
      Signature sig = joinPoint.getSignature();
      logger.info("execute method {}.{} take time: {}(ms)", sig.getDeclaringTypeName(),
          sig.getName(), timeCost);
      
      logger.info("======= after around advice {} ======", joinPoint.getSignature().getName());
    }
    
  }
  
  @AfterThrowing(value = "point()", throwing = "e")
  public void afterThrowingAdvice(JoinPoint joinPoint, Throwable e) {
    logger.info("======= after throwing advice {} ======", joinPoint.getSignature().getName());
    logger.info(" > error info：" + e.getMessage());
  }
  
  @AfterReturning(value = "point()")
  public void afteReturningAdvice(JoinPoint joinPoint) {
    logger.info("======= after returning advice {} ======", joinPoint.getSignature().getName());
  }
  
}
```
