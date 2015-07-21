
categories:
 - git

tags:
 - git
 - 小记

title: git命令小记
---


[TOC]


## 1. git日志格式化
``` shell
git whatchanged -1 --pretty=raw --no-abbrev
git log -1 --pretty=format:'commit %H%ntree %T%nparent %P%nauthor %an <%ae> %at %ar%ncommitter %cn <%ce> %ct %cr%n%n %s%n%n'
```


## 2. 标签按时间排序
``` shell
子模块最新的tag: git submodule foreach 'git tag | xargs -I@ git log --format=format:"%ci %H @%n" -1 @ | sort -r | head -1'
git tag | xargs -I@ git log --format=format:"%ci %h @%n" -1 @ | sort
git tag | xargs -I@ git log --format=format:"%ci %h @%n" -1 @ | sort -r
```


## 3. 重置本地代码
`git reset --mixed`：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
`git reset --soft`：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
`git reset --hard`：彻底回退到某个版本，本地的源码也会变为上一个版本的内容


## 4. 创建分支
git branch develop


## 5. 切换分支
git checkout develop

<!-- more -->

## 6. 查看分支版本
git branch
git branch -v
git branch -r


## 7. 项目多模块更新
git submodule init
git submodule update
git submodule status


## 8. 修改最近一次提交
``` shell
git commit -m 'initial commit'
git add forgotten_file
git commit --amend
```


## 9. git ssh配置
密钥&公钥对生成(默认生成位置为：~/.ssh)：
ssh-keygen -t rsa -C "yeshaoting@163.com"

**说明**：
id_rsa是密钥，如同个人密码一般，别人拿到这个就能以你的身份登录git。
id_rsa.pub是公钥，用于在服务器端注册。当访问远程服务器时，远程服务器会拿着他那端的公钥与本机的密钥匹配，成功即可无密码访问。

测试命令：
> git -T User@Host

### git配置优先顺序
local > global > system
local：项目目录/.git/config
global：当前用户目录/.gitconfig
system：/etc/gitconfig


## 10.配置值获取
config demo：
[user]
        name = shaotingye
        email = shaotingye@sohu-inc.com

获取命令为：
> git config --get user.email


## 11. git 放弃本地修改 强制更新
``` shell
git fetch --all
git reset --hard origin/master
```

参考：http://blog.csdn.net/a06062125/article/details/11727273