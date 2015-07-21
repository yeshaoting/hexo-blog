
categories:
 - linux

tags:
 - linux
 - nginx

title: linux安装nginx
---

Nginx服务器的安装配置


## 1. 下载并解压安装包
下载地址：http://nginx.org/en/download.html

> tar -zxvf nginx-x.x.xx.tar.gz （x.x.xx：为版本号）


## 2. 编译
cd nginx-x.x.xx
./configure --with-http_stub_status_module --prefix=/home/yeshaoting/java/server/nginx/nginx-1.8.0  --without-http_rewrite_module --without-http_gzip_module


[^1]: http://minitoo.blog.51cto.com/4201040/850654