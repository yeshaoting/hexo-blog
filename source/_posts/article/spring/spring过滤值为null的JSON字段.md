title: "\bspring过滤\b\b值为null的JSON\b\b字段"
tags:
  - issue
  - java
  - spring
categories:
  - 后端开发
date: 2016-02-28 16:57:00
---

<img src="/asserts/images/spring.png" class="img-logo img-center" />


## 问题说明
使用@ResponseBody注解的spring接口返回的JSON格式结果有时会返回包含值为null的字段，但是与前端联调可能并不希望包含这样的字段。
因此，需要过滤掉这类字段。


## 解决方案
spring json序列化时，通过`com.fasterxml.jackson.annotation.JsonInclude.Include`指定是否返回值为null的字段。

如下配置所示：
``` xml
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="com.fasterxml.jackson.databind.ObjectMapper">
                    <property name="serializationInclusion">
                        <value type="com.fasterxml.jackson.annotation.JsonInclude.Include">NON_NULL</value>
                    </property>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

## 参考文档
http://segmentfault.com/q/1010000002522525