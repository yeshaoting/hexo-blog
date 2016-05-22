tags:
  - python
categories:
  - 系统脚本
title: python 520
date: 2016-05-22 19:04:00
---

<img src="/asserts/images/logo/python.png" class="img-logo img-center" />

## 一、前言
最近跟老婆大人都在学习python。
正值一年一度的5月20号之际，给老婆的礼物除了发520的红包外，就是这个老婆让我帮她完成的小小python程序了。

## 二、代码
``` python
#!/usr/bin/env python
#coding=utf8


if __name__ == '__main__':
    while True:
        s = raw_input("请输入甜言蜜语：")
        if s == 'exit' or s == 'quit':
            break;

        print '爱的回应：六点就走，回家陪你。'
```


## 三、运行截图
![520运行图](/asserts/images/article/python 520/520_run.png)
