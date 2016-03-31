categories:
  - 系统配置
tags:
  - 五笔
  - ubuntu
title: Linux安装极点五笔的方法
date: 2015-12-29 13:58:00
---


## 一、下载附件文件vissible-ibus.tar.gz
免费下载地址在http://linux.linuxidc.com/
用户名与密码都是www.linuxidc.com
具体下载目录在/pub/2011/10/23/Ubuntu 11.10安装极点五笔/


## 二、安装
``` bash
$ tar xvzf vissible-ibus.tar.gz
$ cd vissible-ibus
$ sudo cp vissible.db /usr/share/ibus-table/tables
$ sudo cp vissible.gif /usr/share/ibus-table/icons
```

<!-- more -->

## 三、删除文件
``` bash
$ rm vissible-ibus.tar.gz
$ rm vissible.db
$ rm vissible.gif
$ rm vissible.txt
```


## 四、启动输入法
在任务栏右键点那个键盘小图标，在下拉菜单选“重新重启”（这个重新启动并不是重并报启动系统，
而是重新启动ibus输入法），然后再右键点那个键盘小图标并在下拉菜单中选“首选项”，然后在弹
出的窗口中点击“输入法”选项卡，接着点“选择输入法”，然后添加极点五笔就可以了。


## 五、修改右shift为中英文切换
把table.py里的Shift_R改为Shift_L
``` bash
$ sudo vim table.py

# Match mode switch hotkey
if not self._editor._t_chars and ( self._match_hotkey (key, keysyms.Shift_L, modifier.SHIFT_MASK + modifier.RELEASE_MASK))

……

if self._match_hotkey (key, keysyms.Shift_R, modifier.SHIFT_MASK + modifier.RELEASE_MASK) and self._ime_py
```


## 六、参考文档
http://hi.baidu.com/hkdog/item/8ae75ea5938e23de5af191ff
