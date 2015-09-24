
categories:
  - cache

tag:
  - redis

title: redis使用手记
---

## 服务监控

## 性能测试

1. redis-cli monitor
使用如下命令查看redis GET/SET请求的频率
redis-cli monitor | grep "GET"
redis-cli monitor | grep "SET"

