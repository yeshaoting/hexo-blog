categories:
  - 后端开发
tags:
  - taglib
  - java
title: 自定义taglib
date: 2015-12-29 15:59:00
---

<img src="/asserts/images/javaee.png" class="img-logo img-center" />


## 一、扩展函数库
在taglib函数类中添加需要的函数randomElement、join等。
``` java
/**
 * @description JSTL function ext
 * 
 * @author yeshaoting
 * @date 2015-06-29 14:52:57
 */
public class FunctionsExt extends Functions {
  
  /**
   * <pre>
   * jstl's fn:join only works with String[]. This one is more general.
   * 
   * usage: ${nc:join(values, ", ")}
   * @see http://stackoverflow.com/questions/3886211/taglib-in-java-tag-with-array-parameter
   * </pre>
   */
  public static String join(Object values, String seperator) {
    if (values == null)
      return StringUtils.EMPTY;
    if (values instanceof Collection<?>)
      return StringUtils.join((Collection<?>) values, seperator);
    else if (values instanceof Object[])
      return StringUtils.join((Object[]) values, seperator);
    else
      return values.toString();
  }
  
  /**
   * get element from elementes by random
   * 
   * @param elements
   * @return
   */
  public static String randomElement(String[] elements) {
    if (ArrayUtils.isEmpty(elements)) {
      return StringUtils.EMPTY;
    }
    
    int index = new Random().nextInt(elements.length);
    return CollectionUtils.get(elements, index).toString();
  }
  
}
```

<!-- more -->


## 二、创建taglib fn-ext.tld
放置文件位置：/martinye-demo-spring-web/src/main/webapp/WEB-INF/taglib/fn-ext.tld

参考fn.tld内容修改：
1. short-name改为我们自己的命令空间fnExt。
2. uri：http://demo.martinye.com/jsp/jstl/functions-ext
3. 定义functions，主要修改name, function-class, function-signature

修改后的完整tld内容如下：
``` xml
<?xml version="1.0" encoding="UTF-8" ?>

<taglib xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-jsptaglibrary_2_0.xsd"
  version="2.0">

  <description>JSTL 1.1 functions ext library</description>
  <display-name>JSTL functions ext</display-name>
  <tlib-version>1.1</tlib-version>
  <short-name>fnExt</short-name>
  <uri>http://demo.martinye.com/jsp/jstl/functions-ext</uri>

  <function>
    <description>
      get element from elementes by random
    </description>
    <name>randomElement</name>
    <function-class>com.martinye.demo.util.taglib.FunctionsExt</function-class>
    <function-signature>java.lang.String randomElement(java.lang.String[])</function-signature>
    <example>
      &lt;c:if test="${fnExt:randomElement(elements)}">
    </example>
  </function>
  
  <function>
    <description>
      Joins all elements of a collection into a string.
    </description>
    <name>join</name>
    <function-class>com.martinye.demo.util.taglib.FunctionsExt</function-class>
    <function-signature>java.lang.String join(java.lang.Object, java.lang.String)</function-signature>
    <example>
      ${fnExt:join(collection, ";")}
    </example>
  </function>

</taglib>
```

###  相关说明
1. 函数式taglib prefix不允许包含特殊字符，标签类taglib可以包含部分特殊字符
实际操作过程中，遇到不允许包含字符：`-`。
例如：matinye-fn或者fn-ext这样的prefix都是不对的。
**注**：有待考证。

2. short-name, uri都要唯一。

3. 当前命令空间fnExt下所有的函数名都不允许重复，即便是重载函数。
例如：join(array)，join(array, String)不能都同时定义在taglib中。

4. 不允许使用java.util.Collection类型作为参数，使用array或者Object代替。
**注**：有待考证。


## 三、web.xml配置taglib
``` xml
  <jsp-config>
    <taglib>
      <taglib-uri>http://demo.martinye.com/jsp/jstl/functions-ext</taglib-uri>
      <taglib-location>/WEB-INF/taglib/fn-ext.tld</taglib-location>
    </taglib>
  </jsp-config>
```

## 四、使用taglib

1. jsp页面定义
``` xml
<%@ taglib uri="http://demo.martinye.com/jsp/jstl/functions-ext" prefix="fnExt"%>
```

2. 页面使用
``` xml
${fnExt:join(cdnServers, " ") }<br/>
<img src="http://${fnExt:randomElement(cdnServerArr) }/resources/img/logo.png" /><br/>
```

## 五、参考文档
[Taglib in Java: tag with array parameter][^1]


[^1]: http://stackoverflow.com/questions/3886211/taglib-in-java-tag-with-array-parameter
