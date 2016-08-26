title: Charles使用手记
tags:
  - charles
  - mac
categories:
  - 工具软件
date: 2016-02-14 18:55:00
---

<img src="/asserts/images/logo/charles.png" class="img-logo img-center" />


## 一、 安装&破解
官方网站下载原版：https://www.charlesproxy.com/latest-release/download.do

破解文件下载：[charles 3.11 mac版 注册码&破解](http://www.typechodev.com/index.php/archives/518/)
适用于Charles3.11.x，含Windows版和Mac版。
使用方法：下载并解压后，选择响应平台的charles.jar并替换安装目录下对应的同名文件即可。替换后记得重启Charles哈。


## 二、设置Wifi热点
1. 在“系统偏好设置”中找到共享，并打开，如下图所示：
![共享]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/234626efeiiiee0ie0de1d.png)
![共享]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/234626efeiiiee0ie0de1d.png)

2. 设置wifi
![设置wifi]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/234626dfzl5ddirvab0wlf.png)
![设置wifi]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/234626dfzl5ddirvab0wlf.png)

3. 设置互联网共享
![互联网共享]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/234626zltkgqegkdk9egk9.png)
![互联网共享]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/234626zltkgqegkdk9egk9.png)

参见：[将你的Mac设置为共享的Wifi热点](http://www.macx.cn/thread-2076838-1-1.html)


<!-- more -->


## 三、代理手机
1. 查看代理ip地址
当前IP地址为192.168.155.43，如下图所示：
![代理ip]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/charles-proxy-local-ip.png)
![代理ip]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/charles-proxy-local-ip.png)

2. 打开charles http代理端口
打开charles http代理端口，默认端口为8888.
![打开charles http代理端口]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/charles-proxy-http-setting.png)
![打开charles http代理端口]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/charles-proxy-http-setting.png)

3. 手机代理配置
![手机代理配置]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/charles-proxy-phone.jpg)
![手机代理配置]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/charles-proxy-phone.jpg)

4. 允许手机访问
![允许手机访问]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/charles-proxy-trust-dialog.png)
![允许手机访问]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/charles-proxy-trust-dialog.png)


## 四、抓包
打开抓包开关：
![开始http抓包]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/charles-proxy-http-switch.png)
![开始http抓包]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/charles-proxy-http-switch.png)

![charles抓取界面]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/1009004.jpg)
![charles抓取界面]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/1009004.jpg)

抓取界面如下图所示：
![数据捕获]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/20141130195559531.png)
![数据捕获]http://o7kubqw1j.bkt.clouddn.com//images/article/charles使用手记/20141130195559531.png)


## 五、注意事项
1. unicode编码
抓到的内容里包含的中文一般都使用unicode进行编码，内容形如：\u65e0\u5e26\u88d9\u7684\u9ad8\u96c5\u88c5\u626e，需要自行解码。

2. ssl支持
charles对https协议的支持还需要特殊配置一下。
需要到官方网站[LEGACY SSL PROXYING页面](http://www.charlesproxy.com/documentation/additional/legacy-ssl-proxying/)下载ssl证书，charles应用以及手机上也需要做一些配置。
如有需要可参考参考文档里的文章进行配置，本文不做介绍。


## 六、参考文档
1. [iOS开发工具——网络封包分析工具Charles](http://www.infoq.com/cn/articles/network-packet-analysis-tool-charles)
2. [charles使用教程指南](http://drops.wooyun.org/tips/2423)
3. [Mac上的抓包工具Charles](http://blog.csdn.net/jiangwei0910410003/article/details/41620363)
4. [Charles Web Debugging Proxy Hacking](http://www.gfzj.us/tech/2015/06/24/charles-hacking.html)
