title: XtraFinder in 10.11
tags:
  - mac
  - xtrafinder
categories:
  - 工具软件
date: 2016-07-18 14:23:00
---
<img src="/asserts/images/logo/mac.png" class="img-logo img-center" />

## 一、背景
MAC系统升级到10.11后，系统提示XtraFinder不可用。



## 二、解决方案
XtraFinder需要注入自己的代码到系统Finder处理过程。
而由于MAC 10.11开始系统默认加入了 SIP(System Integrity Protection) 机制，

### 1. 关闭SIP
重启mac，在开机启动画面长按Command + R，进入恢复模式。
在顶部实用工具菜单，打开Terminal。
输入：`csrutil enable --without debug`，即可。

### 2. 恢复SIP
进入恢复模式，打开Terminal。
输入：`csrutil clear`，即可。


## 三、参考文档
 - [XtraFinder System Integrity Protection](https://www.trankynam.com/xtrafinder/sip.html)
 - [XtraFinder not working on OSX 10.11 El Capitan](https://www.igorkromin.net/index.php/2015/10/06/xtrafinder-not-working-on-osx-1011-el-capitan/)
 - [XtraFinder在 Mac OS 10.11上使用问题](http://www.jianshu.com/p/a47e96645d88)