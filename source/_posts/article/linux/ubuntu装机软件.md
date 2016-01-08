categories:
  - linux
tags:
  - linux
  - ubuntu
  - 软件
title: ubuntu装机软件
date: 2015-12-29 13:58:00
---

修改软件源为163的源。

[TOC]


## 一、应用软件

### 1. 安装vim
命令：
> sudo apt-get install vim


### 2. chrome
直接登录官方网站下载deb包。
https://www.google.com/intl/zh-CN/chrome/browser/

**注**：由于访问google需要翻墙，可以暂时下载百度网盘里的旧版本。


### 3. 截图工具shutter
ubuntu软件中心可下载。

14.04的shutter不提供自定义快捷键，可以利用系统自带的键盘快捷键设置来达到目的。

**设置自定义快捷键方法**：设置 -> 键盘 -> 快捷键 -> 自定义快捷键

命令：
> shutter -s

**注**：系统自带的截图工具也不错。


<!-- more -->

### 4. unity-tweak-tool
unity桌面配置工具, ubuntu软件中心可下载。

命令：
> sudo apt-get install unity-tweak-tool


### 5. 极点五笔输入法
使用系统自带的极点五笔输入法
14.04的ibus与系统软件捆绑，ibus坏了，可能导致系统崩溃。慎装，更加不要做任何删除ibus操作。


### 6. 安装gnome-panel
gnome面板工具，包含 **gnome-desktop-item-edit** 命令，用于创建桌面图标。

命令：
> sudo apt-get install gnome-panel

例如：
> `gnome-desktop-item-edit ~/Desktop/ --create-new`
> gnome-desktop-item-edit ~/Desktop/app.desktop


### 7. 百度准入go-bnac
百度网盘里应该有。
在百度工作专用，用于linux环境下连接百度准入。


### 8. bcompare
安装包，百度网盘有。
**注**：依赖ia32-libs


### 9. RabbitVCS
安装步骤：
> sudo add-apt-repository ppa:rabbitvcs/ppa
> sudo apt-get update
> sudo apt-get install rabbitvcs-core rabbitvcs-cli rabbitvcs-nautilus3 rabbitvcs-gedit

**说明**：重启文档管理器，使用rabbitvcs-nautilus3生效。
> nautilus -q

**注**：不同的系统版本可能需要替换 rabbitvcs-nautilus3 为 rabbitvcs-nautilus 。另外，rabbitvcs-gedit 如无需要的话，可以不安装。

**参考**：[ubuntu软件—RabbitVCS : TortoiseSVN 的替代者][^1]


### 10. VPN工具
优先使用cisco Anyconnect VPN Tool。

如果连接不成功，则可以考虑安装openconnect。
安装命令：
> sudo apt-get install network-manager-openconnect-gnome

连接VPN命令：
> sudo openconnect vpn.sohu-inc.com


### 11. 右键在当前位置打开终端
命令：
> sudo apt-get install nautilus-open-terminal

**注**：安装完成后，需要重启文件管理器
> nautilus -q


### 12. 设置chrome代理
**百度工作**：使用chrome插件 **Proxy SwitchySharp** 加载公司代理配置。
**搜狐工作**：购买红杏网页浏览代理服务，并下载红杏chrome插件使用。


### 13. ubuntu打印机
**百度工作**：
参考http://wiki.babel.baidu.com/twiki/bin/view/Com/Pmo/Iit/Babel/Ubuntu%E5%AE%89%E8%A3%85%E5%85%AC%E5%8F%B8%E6%89%93%E5%8D%B0%E6%9C%BA


### 14. 经典菜单classicmenu-indicator
ubuntu软件中心有下载。

命令：
> `sudo apt-get install --fix-missing classicmenu-indicator`


### 15. rar/zip格式压缩工具
压缩&解压功能，能与ubuntu自带的归档管理器集成。

安装命令：
> sudo apt-get install rar unrar p7zip-full

卸载命令：
> sudo apt-get remove rar unrar p7zip-full


### 16. chmsee
命令：
> sudo apt-get install chmsee

软件源里可能没有chmsee，网上有已经打包好的deb包。
**注**：由于ubuntu14.04软件库未提供命令下载，网上有已经打包好的deb包。百度网盘里有下载好的64位chmsee：chmsee_1.3.1.1-1~getdeb2_amd64.deb

**参考**：[chmsee_1.3.1.1-1~getdeb2_amd64.deb][^2]


### 17. virtualbox
命令：
> sudo apt-get install virtualbox-qt


### 18. LONGENE QQ2013
**前提条件**：64位系统需要安装ia32-libs

安装步骤：
> deb http://archive.ubuntu.com/ubuntu/ raring main restricted universe multiverse
> sudo apt-get update
> sudo apt-get install ia32-libs
> sudo dpkg -i WineQQ2013-20131120-Longene.deb

**注**：WineQQ2012-20121221-Longene.deb已经不让用，而新版本WineQQ2013SP6-20140102-Longene.deb界面又有问题。可以考虑使用webqq + 虚拟机的方式工作。

**参考**：[wine qq 2013 for linux Ubuntu 64位兼容][^3]


### 19. markdown编辑器haroopad
**下载地址**：http://pad.haroopress.com/user.html


### 20. wps-office
**下载地址**：http://linux.wps.cn/

安装字体管理器命令：
> sudo apt-get install font-manager

**注**：字体安装有问题，过程可能需要执行exe程序，比较坑，建议不使用需要的字段。(使用过程不会报错)


### 21. 有道词典
**下载地址**：http://cidian.youdao.com/index-linux.html

**说明**：
使用过程发现会系统出现卡顿的现象。所以，不使用时，最好关闭客户端。
英文发音有问题。


### 22. flashplayer独立播放器
**下载地址**：http://fpdownload.macromedia.com/pub/flashplayer/updaters/11/flashplayer_11_sa.i386.tar.gz


### 23. 切换默认的shell版本
> sudo dpkg-reconfigure dash

**参考**：[Shell编程笔记——Syntax error: "(" unexpected][^5]


### 24. 百度网盘linux版
Debian及基于Debian的发行版(比如ubuntu, linuxmint)请直接下载 bcloud-x.x.x.deb这个包, 然后双击deb包就能安装了;
有些用户并没有安装apt包管理器的GUI界面, 也可以在终端里面安装:
> $ sudo dpkg -i bcloud-x.x.x.deb
> $ sudo apt-get -f install

**参考**：
https://github.com/LiuLang/bcloud
https://github.com/LiuLang/bcloud-packages


### 25. Ubuntu 14.04 64位字体美化(使用文泉驿微黑)
> sudo aptitude install ttf-wqy-microhei

**参考**：[Ubuntu 14.04 64位字体美化(使用文泉驿微黑)][^6]


### 26. 安装gstreamer0.10-ffmpeg
``` shell
sudo add-apt-repository ppa:mc3man/trusty-media
sudo apt-get update
sudo apt-get install gstreamer0.10-ffmpeg
```

**参考**：[Ubuntu 14.04如何轻松启用H.264视频支持][11]



### 27. 安装curl
> sudo apt-get install curl



## 二、开发软件

### 1. 安装git
命令：
> sudo apt-get install git


### 2. securecrt
securecrt(使用securecrt_linux_crack.pl破解时，需要切换到root用户操作：su root)


### 3. subversion
**注**：可能安装rabbitvcs时已经安装

命令：
> sudo apt-get install subversion
> sudo apt-get install libapache2-svn


### 4. 安装mysql
命令：
> sudo apt-get install mysql-server
> sudo apt-get install mysql-workbench


### 5. java
下载jdk bin或者tar.gz包，并安装。

修改java配置 ~/.profile
``` shell
JAVA_HOME=/home/yeshaoting/java/jdk/jdk1.7.0_51
CLASSPATH=$JAVA_HOME/jre/lib
PATH=$PATH:$JAVA_HOME/bin
export JAVA_HOME CLASSPATH PATH
```


### 6. eclipse／STS
**最新的STS下载地址**：http://spring.io/tools/sts/


### 7. maven


## 三、系统配置

### 1. 设置root密码
命令：
> sudo passwd root


### 2. 合上笔记本不休眠
编辑/etc/default/acpi-support
找到内容：LOCK_SCREEN=true，并修改为：LOCK_SCREEN=false


### 3. 中文语言支持


### 4. 用户目录英文名
中文ubuntu里用户目录里的路径改成英文

执行命令：
> export LANG=en_US
> xdg-user-dirs-gtk-update

接下来，会跳出对话框询问：是否将目录转化为英文路径，同意并关闭。
在终端中输入命令：export LANG=zh_CN
关闭终端，并重起。下次进入系统，系统会提示是否把转化好的目录改回中文。
选择不再提示，并取消修改。
主目录的中文转英文就完成了~


### 5. 自动挂载windows分区
查看硬盘的分区情况：
`df -h` 或者 `sudo fdisk -l` 或者使用GParted分区编辑器查看。

编辑/etc/fstab文件，添加如下内容：
``` shell
# auto mount C disk
/dev/sda1 /home/yeshaoting/windows/C ntfs-3g defaults,locale=zh_CN.UTF-8,umask=022 0 0

# auto mount D disk
/dev/sda5 /home/yeshaoting/windows/D ntfs-3g defaults,locale=zh_CN.UTF-8,umask=0,uid=1000,gid=1000 0 0

# auto mount E disk
/dev/sda6 /home/yeshaoting/windows/E ntfs-3g defaults,locale=zh_CN.UTF-8,umask=0,uid=1000,gid=1000 0 0
```

**说明**：umask=022表示只读，umask=0表示可读写，uid和gid设置所属权限（可以通过id username命令）。


### 6. Eclipse设置 Javadoc背景色
命令：
> cd /usr/share/themes/Ambiance/gtk-2.0
> sudo gedit gtkrc

最终12.04及14.04修改gtkrc文件内容如下：
``` shell
gtk-color-scheme = "base_color:#C7EDCC\nfg_color:#4c4c4c\ntooltip_fg_color:#000000\nselected_bg_color:#f07746\nselected_fg_color:#FFFFFF\ntext_color:#3C3C3C\nbg_color:#f6f4f2\ntooltip_bg_color:#f2edbc\nlink_color:#DD4814"
```

**参考**：[Ubuntu 12.04 Eclipse设置 Javadoc背景色][^4]


### 7. thundbird exchange插件ExQuilla



## 四、软件储备

### 1. Ubuntu下的阿里旺旺软件
阿里旺旺的 Ubuntu 下的版本，下载地址是：http://ge.tt/8sPpGIA
**注**：百度网盘里有。

如遇无法登陆的情况，解除登陆限制即可。

**参考**：http://wowubuntu.com/aliwangwang.html


### 3.UltraEdit
下载页面：http://www.ultraedit.com/downloads/uex.html
下载地址：http://www.ultraedit.com/files/uex/Ubuntu/
**注**：百度网盘里有。

#### 关于破解
1. 下载破解器：keygen_ue(百度网盘里有)
2. 删除配置目录：~/.idm/uex/uex.conf

**参考**：
[UltraEdit for Linux破解][^7]
[UltraEdit for linux 不断试用30天的方法 ][^8]


### 4. UltraCompareX
下载页面：http://www.ultraedit.com/downloads/ucx.html
下载地址：http://www.ultraedit.com/files/ucx/Ubuntu/

**注**：百度网盘里有。


### 5. 飞鸽传书GNU/Linux版
命令：
> sudo apt-get install iptux

下载地址：https://code.google.com/p/iptux/downloads/list

**参考**：[信使][^10]


### 6. winusb
安装命令：
``` shell
sudo add-apt-repository ppa:colingille/freshlight
sudo apt-get update
sudo apt-get install winusb
```

**参考**：[WinUSB][^9]


### 7. kwplayer
需要下载 python3-xlib_xx.deb, python3-keybinder_xx.deb, kwplayer_xx.deb 这 三个软件包. 直接双击就能安装deb包.
先安装python3-xlib, 之后是python3-keybinder, 最后是kwplayer.
当然, 也可以在终端里安装, 比如:
``` shell
sudo dpkg -i kwplayer_xx.deb
sudo apt-get -f install
```

**参考**：
https://github.com/LiuLang/kwplayer
https://github.com/LiuLang/kwplayer-packages


### 8. Folder Color
该软件已经集成到Ubuntu 15.04默认库中。
ubuntu14或ubuntu12想要安装此软件的话，需要额外添加软件库。

安装命令如下：
``` shell
sudo add-apt-repository ppa:costales/folder-color
sudo apt-get update
sudo apt-get install folder-color
nautilus -q
```

**参考**：[Folder Color 已添加到 Ubuntu 15.04 默认库中][^12]


### ubuntu截图默认保存位置
**参考**：[Default save directory for gnome-screenshot?][^13]



[^1]: http://www.elain.org/?p=742
[^2]: http://pkgs.org/ubuntu-14.04/getdeb-apps-amd64/chmsee_1.3.1.1-1~getdeb2_amd64.deb.html
[^3]: http://www.longene.org/forum/viewtopic.php?t=4700
[^4]: http://blog.csdn.net/qxb1229/article/details/8265616
[^5]: http://blog.csdn.net/breeze5428/article/details/27353583
[^6]: http://blog.csdn.net/tao_627/article/details/24180781
[^7]: http://mjzhou.com/?p=158
[^8]: http://blog.itpub.net/12216142/viewspace-708801
[^9]: http://wiki.deepin.org/index.php?title=WinUSB
[^10]: http://wiki.deepin.org/index.php?title=%E4%BF%A1%E4%BD%BF
[^11]: http://imcn.me/html/y2014/19624.html
[^12]: http://imcn.me/html/y2015/23176.html
[^13]: http://askubuntu.com/a/134469