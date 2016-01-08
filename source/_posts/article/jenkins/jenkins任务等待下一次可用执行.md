categories:
  - jenkins
tags:
  - jenkins
  - 持续集成
  - 问题
title: jenkins任务等待下一次可用执行
date: 2015-12-29 13:58:00
---

[TOC]

英文：pending-waiting for next available executor

## 问题分析
查阅网上的资料，出现pending-waiting for next available executor的情况主要有二：

### 磁盘存储空间已满
1. jenkins所在的服务器
2. 任务运行涉及到的slave服务器

### 节点离线
jenkins服务正常时，master节点有可能处于离线状态。此时，可以进入系统管理->管理节点，查看一下节点状态。
![节点状态](http://7xkl4i.com1.z0.glb.clouddn.com/jenkins_node_status.png)


## 问题解决

### jenkins所在服务器
重启jenkins服务后，可能需要过段时间节点才会正常加载并工作。

### 任务运行涉及到的slave服务器
重启过程导致slave服务器连接master失败，可以考虑重启一下jenkins slave服务。


## 参考文档
1. http://stackoverflow.com/questions/15112890/jenkins-not-executing-jobs-pending-waiting-for-next-executor
2. http://blog.csdn.net/jinhuiyu/article/details/4827051
