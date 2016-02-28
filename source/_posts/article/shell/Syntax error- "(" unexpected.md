title: 'Syntax error- "(" unexpected'
tags:
  - issue
categories:
  - shell
date: 2016-02-28 16:56:00
---

## 一、问题描述
假如我们在shell文件定义了一个数组pid=(0 0 0 0)，运行文件后则会收到报错：Syntax error: "(" unexpected。


## 二、原因
主要是因为Linux系统shell版本不兼容引起的。 shell的版本有sh,ksh,csh, bash，dash……等等。用命令ls -al /bin/sh可以得到我们当前所用的Linux系统的shell属于何版本。


## 三、解决
通过将当前通过以下方式可以使 shell 切换回 bash：
`sudo dpkg-reconfigure dash`
然后选择 no 或者“否 ”，并确认。这样做将重新配置 dash，并使其不作为默认的 shell 工具。


## 四、参考文档
[Shell编程笔记——Syntax error: "(" unexpected](http://blog.csdn.net/breeze5428/article/details/27353583)
http://ask.chinaunix.net/question/974
http://bbs.csdn.net/topics/390132876