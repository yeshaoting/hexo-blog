categories:
  - cache
tags:
  - redis
title: redis使用手记
date: 2015-12-29 13:58:00
---

# redis使用手记

## 服务监控

## 性能测试

1. redis-cli monitor
使用如下命令查看redis GET/SET请求的频率
redis-cli monitor | grep "GET"
redis-cli monitor | grep "SET"

