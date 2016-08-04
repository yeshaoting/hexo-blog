title: request entity too large
tags:
  - java
categories:
  - 后端开发
date: 2016-07-18 14:24:00
---


<img src="/asserts/images/logo/javaee.png" class="img-logo img-center" />

## 一、问题描述
拍照上传图片时，系统提示: request entity too large，错误码为413。


## 二、问题分析
翻阅http状态码查看413解释如下:

> 413 Request entity too large
The request is larger than what the server is able to process.

由此可知，应该是上传照片超过服务器的限制。



## 三、解决方案

### 1. 解除域名服务器限制
一般情况下，域名服务器选用apache和nginx比较多。

#### apache
对apache服务器而言，相关参数为: `LimitRequestBody`
默认值为0，表示最大2GB。

官方解释如下:
```
Description:	Restricts the total size of the HTTP request body sent from the client
Syntax:	LimitRequestBody bytes
Default:	LimitRequestBody 0
Context:	server config, virtual host, directory, .htaccess
Override:	All
Status:	Core
Module:	core
```

<!-- more -->

> This directive specifies the number of bytes from 0 (meaning unlimited) to 2147483647 (2GB) that are allowed in a request body.
The LimitRequestBody directive allows the user to set a limit on the allowed size of an HTTP request message body within the context in which the directive is given (server, per-directory, per-file or per-location). If the client request exceeds that limit, the server will return an error response instead of servicing the request. The size of a normal request message body will vary greatly depending on the nature of the resource and the methods allowed on that resource. CGI scripts typically use the message body for retrieving form information. Implementations of the PUT method will require a value at least as large as any representation that the server wishes to accept for that resource.
This directive gives the server administrator greater control over abnormal client request behavior, which may be useful for avoiding some forms of denial-of-service attacks.
If, for example, you are permitting file upload to a particular location, and wish to limit the size of the uploaded file to 100K, you might use the following directive: LimitRequestBody 102400

#### nginx
对nginx服务器而言，相关参数为: `client_max_body_size` 
默认值为1M。

官方解释如下:
```
Syntax:		client_max_body_size size;
Default:	client_max_body_size 1m;
Context:	http, server, location
```
> Sets the maximum allowed size of the client request body, specified in the “Content-Length” request header field. If the size in a request exceeds the configured value, the 413 (Request Entity Too Large) error is returned to the client. Please be aware that browsers cannot correctly display this error. Setting size to 0 disables checking of client request body size.

### 2. 解除应用服务器限制
java的应用程序的话，目前使用较多的还是tomcat和jetty两类服务器。
由于遇到的问题是nginx默认限制文件上传大小为1M导致，此类情况暂时不深研。

### 3. 解除应用自身限制
spring容器注入multipartResolver
如下设置，默认不限制文件上传大小。
如果需要做限制的话，可以设置maxUploadSize参数，用来限制文件上传大小(单位:bytes)。
``` xml
<bean id="multipartResolver"
          class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>
```


## 四、参考文档
1. [http://craftcms.stackexchange.com/a/2330](http://craftcms.stackexchange.com/a/2330)
2. [limitrequestbody](http://httpd.apache.org/docs/2.0/mod/core.html#limitrequestbody)
3. [client_max_body_size](http://nginx.org/en/docs/http/ngx_http_core_module.html#client_max_body_size)