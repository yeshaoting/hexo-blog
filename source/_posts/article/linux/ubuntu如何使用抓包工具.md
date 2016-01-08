categories:
  - linux
tags:
  - linux
  - ubuntu
  - 抓包
title: ubuntu如何使用抓包工具
date: 2015-12-29 13:58:00
---

## 借助fiddler

**参考**：[回归 Windows 下的 Fiddler][^1]

要说起抓包工具，当算顶顶大名的fiddler。
只是fiddler程序依赖net framework，而非跨平台工具，只能在window环境下进行抓包。

为了完成ubuntu的浏览行为被window下的fiddler抓包，可以借助虚拟机，里面安装window系统，并安装fiddler，然后开启fiddler的代理功能。


### 设置fiddler代理

进入fiddler options，点击Connections选项卡。
勾选 **Allow remote computers to connect**，并设置 fiddler listens on port值为8080。
如图所示：
![fiddler代理设置](http://7xkl4i.com1.z0.glb.clouddn.com/fiddler%20proxy%20settings.png)


### ubuntu使用fiddler代理
打开ubuntu的网络设置，网络代理处添加HTTP代理。

如图所示：
![ubuntu使用fiddler代理](http://7xkl4i.com1.z0.glb.clouddn.com/ubuntu使用fiddler代理.png)


### 补充说明:虚拟机网络连接

**参考**：[VirtualBox虚拟机网络连接设置的四种方式][^2]

如果使用虚拟机里的fiddler，则需要注意网络的连接方式为"桥接网卡"方式，这样虚拟机里的系统能分配到一个IP地址，主机能通过这个IP地址及对应的端口访问到fiddler开启的代理。

其实，只要主机能通过虚拟机IP ping通即可。


## 借助charles




[^1]: http://imququ.com/post/user-fiddler-on-macos.html#toc-4
[^2]: http://pengranxiang.iteye.com/blog/715829