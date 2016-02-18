title: git多仓库配置
tags:
  - 多仓库
categories:
  - git
date: 2016-02-18 12:32:00
---

## 一、密钥选择
默认情况下，git会使用id_rsa与id_rsa.pub的密钥对，与git代码服务器进行通信。但是，对于同一台机器需要与不同的git代码服务器进行通信的需求，则需要进行特别指定。

例如：需要针对于github使用非默认的密钥对，则可向~/.ssh/config添加如下配置：
``` bash
Host github.com
HostName github.com
User git
IdentityFile /Users/yeshaoting/.ssh/id_rsa_github
```


## 二、用户配置
代码提交的用户信息。

### 1. 全局配置
``` bash
git config --global user.name "叶绍亭"
git config --global user.email "yeshaoting@meituan.com"
```

会在用户目录生成或修改~/.gitconfig文件。
默认情况下，会使用此文件里的配置内容。


### 2. 仓库配置
``` bash
git config user.name "yeshaoting"
git config user.email "yeshaoting@163.com"
```

会修改当前仓库.git/config文件内容。
此文件里的配置会覆盖全局配置里相同的配置内容。
