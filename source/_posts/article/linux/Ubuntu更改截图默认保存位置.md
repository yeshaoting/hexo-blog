categories:
  - linux
tags:
  - linux
  - ubuntu
title: Ubuntu更改截图默认保存位置
date: 2015-12-29 13:58:00
---

## 图形界面

安装dconf-editor
> sudo apt-get install dconf-editor

打开dconf-editor，进入org -> gnome -> gnome-screenshot，修改auto-save-directory值为 file:///home/yeshaoting/Pictures/screenshot/。


## 命令行
> gsettings set org.gnome.gnome-screenshot auto-save-directory "file:///home/yeshaoting/Pictures/screenshot/"

参考文档：[Default save directory for gnome-screenshot?](http://askubuntu.com/a/134469)