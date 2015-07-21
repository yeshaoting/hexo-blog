
categories:
 - linux

tags:
 - linux
 - 五笔

title: Linux安装极点五笔的方法
---

参考：http://hi.baidu.com/hkdog/item/8ae75ea5938e23de5af191ff


## 1.下载附件文件vissible-ibus.tar.gz
免费下载地址在http://linux.linuxidc.com/
用户名与密码都是www.linuxidc.com
具体下载目录在/pub/2011/10/23/Ubuntu 11.10安装极点五笔/


## 2.安装
$ tar xvzf vissible-ibus.tar.gz
$ cd vissible-ibus
$ sudo cp vissible.db /usr/share/ibus-table/tables
$ sudo cp vissible.gif /usr/share/ibus-table/icons


<!-- more -->

## 3.删除文件
$ rm vissible-ibus.tar.gz
$ rm vissible.db
$ rm vissible.gif
$ rm vissible.txt


## 4.启动输入法
在任务栏右键点那个键盘小图标，在下拉菜单选“重新重启”（这个重新启动并不是重并报启动系统，
而是重新启动ibus输入法），然后再右键点那个键盘小图标并在下拉菜单中选“首选项”，然后在弹
出的窗口中点击“输入法”选项卡，接着点“选择输入法”，然后添加极点五笔就可以了。


## 5.修改右shift为中英文切换
$ sudo vim table.py
/match Match mode switch hotkey

\# Match mode switch hotkey
if not self._editor._t_chars and ( self._match_hotkey (key, keysyms.Shift_L, modifier.SHIFT_MASK + modifier.RELEASE_MASK)):
把Shift_L 改为Shift_R
/match because we ignore all Release event below

\# because we ignore all Release event below.
if self._match_hotkey (key, keysyms.Shift_R, modifier.SHIFT_MASK + modifier.RELEASE_MASK) and self._ime_py:
把Shift_R 改为Shift_L
