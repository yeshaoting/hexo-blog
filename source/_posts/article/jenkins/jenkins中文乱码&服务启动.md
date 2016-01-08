categories:
  - jenkins
tags:
  - jenkins
  - 持续集成
  - 问题
title: 'jenkins中文乱码&服务启动'
date: 2015-12-29 13:58:00
---


## 问题描述
项目构建**最新修改记录**中文乱码。


## 问题查看与分析
首先，查看jenkins管理界面的**系统属性**功能。
对应URL地址为：http://jenkins.go.sohuno.com/systemInfo

可以看到jenkins启动环境变量file.encoding值为默认的**ISO-8859-1**(Latin-1字符集)。
不希望jenkins乱码，需要设置其编码方式为UTF-8。(参考：[Jenkins控制台中乱码问题][^1])


## 问题解决
当前jenkins启动命令为：
``` shell
java -Djava.awt.headless=true -DJENKINS_HOME=/var/lib/jenkins -jar /usr/lib/jenkins/jenkins.war --logfile=/var/log/jenkins/jenkins.log --webroot=/var/cache/jenkins/war --httpPort=8080 --ajp13Port=8009 --debug=5 --handlerCountMax=100 --handlerCountMaxIdle=20
```

在启动jenkins时，需要额外添加参数**file.encoding**，并设置其参数值为UTF-8。
修改后的启动命令如下：
``` shell
java -Djava.awt.headless=true -DJENKINS_HOME=/var/lib/jenkins -jar /usr/lib/jenkins/jenkins.war --logfile=/var/log/jenkins/jenkins.log --webroot=/var/cache/jenkins/war --httpPort=8080 --ajp13Port=8009 --debug=5 --handlerCountMax=100 --handlerCountMaxIdle=20 -Dfile.encoding=UTF-8
```


### 扩展说明
由jenkins启动命令可看出：

1. jenkins安装目录
/var/lib/jenkins

2. jenkins服务部署代码
/usr/lib/jenkins/jenkins.war

3. 运行日志
/var/log/jenkins/jenkins.log

4. jenkins代码部署位置
/var/cache/jenkins/war

5. jenkins服务端口
http: 8080
ajp13: 8009

6. jenkins servlet容器
jenkins服务运行在winstone servlet容器，类似于tomcat, jetty等。
jenkins命令启动接收的参数`--logfile`, `--webroot`, `--httpPort`, `--ajp13Port`等都是winstone的。
另外，查看了webroot参数对应的目录，发现目录包含类库**winstone.jar**，也可知jenkins可能是放在winstone容器里运行的。

关于winstone可参考：[Winstone Servlet Container v0.9.10][^6]



[^1]: http://blog.csdn.net/hunterno4/article/details/38424715
[^2]: http://lee2013.iteye.com/blog/2108612
[^3]: http://zhidao.baidu.com/question/177248555728533364.html
[^4]: http://lee2013.iteye.com/blog/2108612
[^5]: http://www.xuebuyuan.com/2108588.html
[^6]: http://winstone.sourceforge.net/

