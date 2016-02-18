categories:
  - git
title: git拉取submodule
tags:
  - submodule
date: 2015-12-29 13:58:00
---

## 一、执行步骤

### 1. 添加子模块配置
子模块的URL加入到.git/config
git submodule init

### 2. 查看模块状态
git submodule status

### 3. 拉取子模块
克隆子模块的仓库和签出父项目中指定的那个版本
git submodule update

### 4. 切换子模块版本
git submodule foreach git checkout develop


## 二、参考文档
[Git Community Book 中文版 - 子模块](http://gitbook.liuhui998.com/5_10.html)
