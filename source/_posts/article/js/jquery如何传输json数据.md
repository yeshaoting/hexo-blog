title: jquery如何传输json数据
tags:
  - jquery
categories:
  - 前端设计
date: 2016-04-10 20:19:00
---

<img src="/asserts/images/logo/jquery.png" class="img-logo img-center" />


## 一、前言
本文阐述基于如下mock接口：
接口：http://openapi.tencentyun.com/v3/user/delete

| 参数 | 参数说明 |
|---|---|
| appid | 应用ID |
| appkey | 应用密钥 |
| userid | 用户ID |
| operid | 操作者ID |


## 二、请求删除用户

### 1. 构造请求参数
``` javascript
var params = {
    "appid": "2",
    "appkey": "5F154D7D2751AEDC8527269006F290F70297B7E54667536C",
    "userid": "B624064BA065E01CB73F835017FE96FA",
    "operid": "B624064BA065E01CB73F835017FE96FA"
};
```

<!-- more -->

### 2. 构造请求
``` javascript
var url = "http://openapi.tencentyun.com/v3/user/delete";
$.ajax({
    type: "POST",
    url: url,
    async: false,
    processData: false,
    data: JSON.stringify(params),
    contentType: "application/json",
    dataType: 'json',
    success: function(json) {
        console.log(json);
    }
});
```

### 3. 需要注意几个地方
- contentType设置为：application/json
- dataType设置为：json
- data必须是字符串，而不是一个js对象


### 4. json对象转字符串
- 可以对于js自带的JSON.stringify(params)转成字符串
- 使用jquery的$.param()方法将json对象序列化成键值对


## 三、参考文档
[How can I use JQuery to post JSON data?](http://stackoverflow.com/questions/6255344/how-can-i-use-jquery-to-post-json-data/6255394#6255394)