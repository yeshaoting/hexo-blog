title: Connection closed by remote host
tags:
  - ssh
  - issue
categories:
  - tool
date: 2016-02-28 16:23:00
---
## 异常内容
ssh_exchange_identification - Connection closed by remote host


## 调试
用ssh -v去连有问题的服务器，这时会有比较详细的调试信息在屏幕上输出，可以帮助判断是哪一步出了问题。


## 参考文档
http://www.zhihu.com/question/20023544