title: WEB安全渗透分享
tags:
  - 渗透
categories:
  - 信息安全
date: 2016-03-15 18:08:00
---
<img src="/asserts/images/logo/hack.png" class="img-logo img-center" />


WEB安全渗透靶场：http://10.4.236.115:8080/
是否开放所有关卡切换：http://10.4.236.115:8080/sohu/switchOpenStage


## 0x00 前言
此次分享内容旨在加深开发工作中的安全意识，也希望能起到抛砖引玉的作用，在分享完成后的讨论环节大家也说说自己现在或者曾经遇到过的安全问题。

为了让大家能参与其中，对WEB安全问题有个更具象的认识。周末二天基于 `angularjs`、`jquery`、`spring`、`spring mvc`、`mybatis`、`mysql` 搭建一个比较粗糙的WEB安全靶场，复刻自参加的第一屇搜狐安全擂台赛，供大家hack。
如发现问题，请各位见谅哈。


### 1. 用户登记
主要是为了收集用户名信息，进行登记，用于记录闯关进度!

界面如下图所示：
![用户登记](/asserts/images/article/WEB安全渗透分享/Snip20160308_1.png)


### 2. 用户主页
简单地展示用户当前进度，并提供进入下一关的入口。后续可以扩展，显示击败百分之多少的对手。

界面如下图所示：
![用户主页](/asserts/images/article/WEB安全渗透分享/Snip20160308_3.png)


<!-- more -->

## 0x01 关卡1
属于热身题，纯属练手，引导用户在之后的关卡中找到更多破解信息。

### 1. 提示
提示如下：
> 任务目标：找出正确的用户名与密码并通过验证。
Tips：这个页面用户与密码不需要去服务器交互验证。

### 2. 界面
界面如下图所示：
![关卡1](/asserts/images/article/WEB安全渗透分享/Snip20160308_4.png)

### 3. 揭密
用户名密码直接写在了页面的js逻辑里，跟随页面元素js操作即可找到对应的内容。
![关卡1-页面代码](/asserts/images/article/WEB安全渗透分享/Snip20160308_5.png)
![关卡1-js代码](/asserts/images/article/WEB安全渗透分享/Snip20160308_6.png)

由此，可见第一关的用户名密码如下：
``` javascript
var stage1Username = "stage1_user";
var state1Password = "tHis1Is3aSim3p#ab$";
```

### 4. 小结
一般不会出现此类小白程序员的情况。


## 0x02 关卡2
这是一道SQL注入题。

SQL注入攻击（SQL Injection），简称注入攻击，是Web开发中最常见的一种安全漏洞。可以用它来从数据库获取敏感信息，或者利用数据库的特性执行添加用户，导出文件等一系列恶意操作，甚至有可能获取数据库乃至系统用户最高权限。

### 1. 提示
提示如下：
> 任务目标：本题没有可登入的用户名密码，请绕过登录验证。
Tips：就算是判断了用户名 and 密码 都正确，也是被禁用的。

### 2. 界面
界面如下图所示：
![关卡2](/asserts/images/article/WEB安全渗透分享/Snip20160308_7.png)

### 3. 揭密
对于sql而言，有些特殊字符（如：单引号，--，\，#等）、保留字（如：select，values，delete等）和内置函数（date，time，concat等），使用不当都有可能会导致SQL执行错误。

由此，我们尝试使用单引号，发现登录异常提示如下：
![特殊字符注入](/asserts/images/article/WEB安全渗透分享/Snip20160308_12.png)

并且，发现在控制台出现如下异常信息：
``` bash
StatementCallback; bad SQL grammar [select count(id) from user2 where username = ''' and password = '']; nested exception is com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '''' and password = ''' at line 1
```

从异常内容可以发现，出错的SQL内容为：
``` sql
select count(id) from user2 where username = ''' and password = ''
```

于是乎就从盲目地注入，变成了有针对性的注入。
针对于此，可以填写如下内容进入SQL注入：
``` sql
username: \
password: or '' = '

username: \
password: or 1 != '

username: ' or 1 !=
password: '--

username: ' or 1 !=
password: '--

username: ' or 1=1 -- x
username: ' or 1=1 #
```

**注**：答案可以很多。


### 4. 小结
- 避免出现一些详细的错误消息，异常不要抛出来，在服务器端打印日志用于异常分析就好。
- 永远不要使用动态拼装sql，使用数据库或开发框架提供的参数化查询接口。
- 机密信息不要直接存放，加密或者hash密码和敏感的信息再存储。
- 用户输入过滤：对特殊字符单引号和双"-"等进行转义处理或编码转换。
- SQL注入检测工具进行检测。如：sqlmap、SQLninja等。


### 5. 参考文档
- [SQL 注入](http://php.net/manual/zh/security.database.sql-injection.php)
- [SQL注入技术](http://baike.baidu.com/view/3896.htm#3)
- [9.4 避免SQL注入](https://github.com/astaxie/build-web-application-with-golang/blob/master/zh/09.4.md)


## 0x03 关卡3

### 1. 提示
提示如下：
> 任务目标：找到通关后台，并将自己的通关数目加一级
Tips：答案不在本页

### 2. 界面
界面如下图所示：
![关卡3](/asserts/images/article/WEB安全渗透分享/Snip20160308_13.png)

### 3. 揭密
由提示找到通关后台可知，找到后台是关键。
一般地，常规后台命名为admin、admin\index、manage、dashboard等。

尝试如下地址：http://localhost:8080/sohu/index#/admin
于是进入了后台管理界面，如下图所示：
![WEB安全渗透实战-后台管理系统
](/asserts/images/article/WEB安全渗透分享/Snip20160308_15.png)
![后台管理系统-升级](/asserts/images/article/WEB安全渗透分享/Snip20160308_16.png)

尝试点击**升级**直接通关，其他所有内容其实都是假的。


### 4. 小结
后台弱口令


## 0x04 关卡4
将非最高价格物品价格修改为负值，从而增加自己的虚拟货币，然后再买最高价格物品。

### 1. 提示
提示如下：
> 任务目标：通过本页面提供的虚拟货币购买价值最高的物品
Tips：0 - (-1) = 1

### 2. 界面
界面如下图所示：
![关卡4](/asserts/images/article/WEB安全渗透分享/Snip20160309_21.png)

### 3. 揭密
由于初始时每人只拥有5000000的虚拟货币，完全不只以购买价值最高的物品。
因为要购买前需要增加自己的虚拟货币。

再根据下面的提示说0 - (-1) = 1，可与购买联想到：购买物品时，修改传到后端的价格为负数，来达到为自己增加虚拟货币的目的。

于是我们尝试购买价格为399的物品。系统提示：购买成功，但是未购买价值最高的物品，如下图所示：
![未购买最高价值的物品](/asserts/images/article/WEB安全渗透分享/Snip20160309_22.png)

我们再通过console可以看到，发出的POST请求为：http://10.4.236.115:8080/sohu/stage4/pass?id=1&price=399

查看购买的js请求代码如下：
![关卡4-js代码](/asserts/images/article/WEB安全渗透分享/Snip20160309_23.png)

在下一次购买请求时，设置price值为 -7999999 。请求完成后，依然提示购买失败的消息，但是虚拟货币却变成了：12999600。

于是，我们就可以开开心心地购买最高价格的物品了。

完成的js操作如下所示：
``` javascript
// 增加虚拟货币
var url = 'http://10.4.236.115:8080/sohu/stage4/pass?id=1&price=-7999999';
$.ajax({
    method: 'post',
    async: false,
    url: url,
    success: function(data) {
        if (data.status == 200) {
            console.log('SUCCESS');
        }
    }
});

// 购买最高价格物品
var url = 'http://10.4.236.115:8080/sohu/stage4/pass?id=3&price=7999999';
$.ajax({
    method: 'post',
    async: false,
    url: url,
    success: function(data) {
        if (data.status == 200) {
            console.log('SUCCESS');
        }
    }
});
```

### 4. 小结
虽然做了不能够买比自己持有总金额高的物品的检验，但是未校验物品金额不允许为负数，也未校验id对应的物品金额与实际金额是否相符。
对于关键逻辑，对参数做更加严格的校验。


## 0x05 关卡5
暴力破解

### 1. 提示
提示如下：
> 任务目标：通过收到的短信验证码完成手机验证。
Tips：有时候采用最原始的方法收到的效果反而更显著。

### 2. 界面
界面如下图所示：
![关卡5](/asserts/images/article/WEB安全渗透分享/Snip20160309_24.png)

### 3. 揭密
从Tips里提到的最原始的方法，大概能猜测到最直接的破解方式：**字典破解** 或 **暴力破解**。
要么验证码是个弱密码，要么验证码很方便枚举出来。

接下来，点击“获取短信验证码”，提示：新的3位数字验证码已经通过短信，发送到13800138000的手机上，请注意查收！
因此，可见验证码可以枚举出来，只需要发起不超过1000次请求即可找到验证码。

看过js请求代码，了解请求构造后，可以使用如下操作暴破：
``` javascript
var url = 'http://10.4.236.115:8080/sohu/sms/code/verfiy';
var len = 1000;
for (var i = 0; i < len; i++) {
	$.ajax({
		method: 'post',
        async: false,
		url: url + '?phone=13800138000&code=' + i,
		success: function(data) {
			if (data.status == 200) {
				console.log('code:' + i);
				i = len;
			}
		}
	});
}
```

暴破完成后，会打印出对应的验证码，之后即可通过dashboard页直接进入下一关。

**注**：考虑验证码可能会补扩散开，因此每次有人过关，验证码就会重新刷新一次。

### 4. 小结
尽量不要设置容易枚举的密码或验证码。同时，对于敏感信息可以加上一些限制，类似于：时效性限制或者一天内允许尝试失败次数等。


## 0x06 关卡6
读取服务器上的文件

### 1. 提示
提示如下：
> 任务目标：找到可以通过本关的密码并提交。
Tips：密码被硬编码到了当前页面服务器上的php代码里。

### 2. 界面
界面如下图所示：
![关卡6](/asserts/images/article/WEB安全渗透分享/Snip20160309_25.png)

### 3. 揭密
老规范，查看页面代码，如下图所示：
![关卡6-页面代码](/asserts/images/article/WEB安全渗透分享/Snip20160309_27.png)

再结合Tips说密码被放在了php代码里。巧的是页面里的图片也是通过后缀为php的url生成的。
由此可知，这个php页面就是stage6.php。如此同时，这个服务还支持file参数。

于是，可以尝试设file值为../stage6.php、stage6.php、sohu/stage6.php、../sohu/stage6.php等等获取php代码内容。

最后发现该php代码文件获取链接地址为：http://10.4.236.115:8080/sohu/stage6.php?file=stage6.php

查看页面内容如下图所示：
![关卡6-php代码](/asserts/images/article/WEB安全渗透分享/Snip20160309_28.png)

额，其实这是一个jsp页面，只是以jsp为后缀的url被jetty使用默认的servlet自动转发了，不得以只好用php做为后缀用了。

总之，最后能获取到了密码为：sATa3HGe6

### 4. 小结
不要以实际文件路径为文件请求参数。
如果服务需要的文件可枚举，则可以在后端限制可获取文件范围；或者把所有需要的文件放置于一个特定目标，只允许访问通过服务获取此目标下的文件。


## 0x07 关卡7
伪造ip地址

### 1. 提示
提示如下：
> 任务目标：请获得本页面的访问权限
Tips：一会公开的来源控制方法也许并不安全

### 2. 界面
界面如下图所示：
![关卡7](/asserts/images/article/WEB安全渗透分享/Snip20160309_29.png)

### 3. 揭密
点击“验证权限”发现系统给出如下提示：
![关卡7-提示](/asserts/images/article/WEB安全渗透分享/Snip20160309_30.png)

提示内容：你不是来自于127.0.0.1的访客。反过来，提示的意思可以理解为只有IP地址来自于127.0.0.1的用户才有访问的权限。

于是则需要伪造请求ip地址为127.0.0.1。
一般地，涉及到ip地址的请求头有：X-Real-IP、X-Forwarded-For、Host。
伪造ip时，一一尝试，最终发现设置 X-Forwarded-For 为127.0.0.1 成功通关。

### 4. 补充内容

#### 请求头信息修改工具
使用chrome插件工具ModHeader来修改请求头信息：https://chrome.google.com/webstore/detail/modheader/idgpnmonknjnojddfkpgkljpfnnfcklj

#### 客户端IP流转
服务器经过Apache,Squid等反向代理软件层层代理转发后，其原始IP地址可能会丢失。

服务器通过`request.getRemoteAddr();`只能获取直接请求的ip地址。
对于服务器之上架设有代理服务器的，则是代理服务器的ip地址；对于用户直接访问服务器，中间并没有代理服务器的情况，获取的ip地址才是用户真正的ip地址。

对于服务器代理产生的ip丢失情况，一些反射代理服务器开始引入X-Forwarded-For和X-Real-IP等非正式的http协议头，用于保存原始客户端的ip地址。

一般地，服务器程序会优先从X-Forwarded-For和X-Real-IP二个协议头里取ip地址，没有再从request里取ip地址。
**注**：CRM后端代码还是额外check Proxy-Client-IP和WL-Proxy-Client-IP协议头。

参考：[Java获取客户端IP](http://www.cnblogs.com/ITtangtang/p/3927768.html)

### 5. 小结
大家通用的客户端ip地址获取方法，经过代理服务器转发后，并不一定可靠。
因此，使用ip来做权限上的限制也并不是100%安全的。


## 0x08 关卡8

### 1. 准备

#### 二维码生成
生成二维码工具：
- http://cli.im/
- http://liantu.com/

生成内容：
http://10.4.236.115:8080/sohu/stage7?level=7&username=yeshaoting&time=1457452800&verify=c6ce17b02eb15fd1831267a8736bd933

#### 二维码解码
推荐使用：http://tool.oschina.net/qr?type=2
试过其他的几个不能解码中间有logo图片的二维码。

### 2. 提示
提示如下：
> 任务目标：请主动设置你的过关级别为8
Tips：你猜到他们猜不到你猜得到他们猜不到你猜得到

### 2. 界面
界面如下图所示：
![关卡8](/asserts/images/article/WEB安全渗透分享/Snip20160310_35.png)

### 3. 揭密
第8关上来就是一个大大的二维码。
通过二维码反解工具，得到一个网址：http://10.4.236.115:8080/sohu/stage7?level=7&username=yeshaoting&time=1457452800&verify=c6ce17b02eb15fd1831267a8736bd933。

由于我们第7关已经过关，因此这个url还需要变通一下。修改后完成的url为：http://10.4.236.115:8080/sohu/stage8?level=8&username=yeshaoting&time=1457452800&verify=c6ce17b02eb15fd1831267a8736bd933。

直接提示："这是一次已经过期的提交，请在2分钟内完成请求提交。"
由此可见，参数time不在2分钟内。反查查秒级时间得到内容为：2016-03-09 00:00:00，确认不是当时时间2分钟内。

修改time内容为当前时间后再提交，提示："请求在服务器端验证失败。"。
可以猜测，参数verify不对。这个verfiy一看就是经过md5加密的。

根据开发时与第三方接口对接的经验，verfiy是通过前面几个参数按照某种规则生成的。
如是，依次尝试主流的微信、腾讯、淘宝开放平台，最终发现与腾讯开放平台的签名规则相似。

第1步：将level、username、time三个参数进行字典序排序。
第2步：将第1步中排序后的参数(key=value)用&拼接起来：level=8&time=1457452800&username=yeshaoting。
第3步：对第2步内容进行md5加密，得到verfity值。


### 4. 补充知识：签名规则

#### 微信开放平台
http://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421135319&token=&lang=zh_CN
![关卡8-微信签名规则](/asserts/images/article/WEB安全渗透分享/Snip20160310_31.png)

#### 腾讯开放平台
http://wiki.open.qq.com/wiki/%E8%85%BE%E8%AE%AF%E5%BC%80%E6%94%BE%E5%B9%B3%E5%8F%B0%E7%AC%AC%E4%B8%89%E6%96%B9%E5%BA%94%E7%94%A8%E7%AD%BE%E5%90%8D%E5%8F%82%E6%95%B0sig%E7%9A%84%E8%AF%B4%E6%98%8E
![关卡8-腾讯签名规则](/asserts/images/article/WEB安全渗透分享/Snip20160310_33.png)

#### 淘宝开放平台
http://open.taobao.com/doc2/detail.htm?articleId=101617&docType=1&treeId=1#s5
![关卡8-淘宝签名规则](/asserts/images/article/WEB安全渗透分享/Snip20160310_34.png)

### 5. 小结
签名方式尽量与市面上的一些规则区分开。
签名规则也是有可能会被猜出，可以通过一些别的类似于关卡5小结里的限制来辅助提高安全性。