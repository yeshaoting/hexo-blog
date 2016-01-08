categories:
  - git
tags:
  - git
title: 推送本地git仓库
date: 2015-12-29 13:58:00
---

本文参考：[Create Local Git Repo & Push to GitHub][1]

[TOC]


## 操作步骤


### 一、打开终端

### 二、到本地文件目录
 > cd /home/developer/public_html

### 三、配置git
用户名：
 > `git config --global user.name "叶绍亭"`

邮箱：
 > `git config --global user.email "yeshaoting@163.com"`

配色方案：
 > `git config --global color.ui auto`

### 四、 项目初始化
 > git init

初始化成功会看到如下内容：
``` shell
"Initialized empty Git repository in home/developer/public_html/.git"
```

<!-- more -->

### 五、验证初始化后的状态
 > git status

应该能看到类似如下的信息：
``` shell
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#   .project
#   bootstrap/
#   index.html
nothing added to commit but untracked files present (use "git add" to track)
```

### 六、添加文件或者目录到索引(index)
 > git add *

应该能看到类似如下的信息：
``` shell
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#   new file:   bootstrap/css/bootstrap-theme.css推送本地git仓库
#   new file:   bootstrap/css/bootstrap-theme.min.css
#   new file:   bootstrap/css/bootstrap.css
#   new file:   bootstrap/css/bootstrap.min.css
#   new file:   bootstrap/css/custom.css
#   new file:   bootstrap/css/offcanvas.css
#   new file:   bootstrap/fonts/glyphicons-halflings-regular.eot
#   new file:   bootstrap/fonts/glyphicons-halflings-regular.svg
#   new file:   bootstrap/fonts/glyphicons-halflings-regular.ttf
#   new file:   bootstrap/fonts/glyphicons-halflings-regular.woff
#   new file:   bootstrap/js/bootstrap.js
#   new file:   bootstrap/js/bootstrap.min.js
#   new file:   bootstrap/js/holder.js
#   new file:   index.html
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#   .project
```

### 七、提交索引内容到仓库(repository)
 > git commit -m "My first commit"

应该能看到类似如下的信息：
``` shell
[master (root-commit) c43daef] My first commit
14 files changed, 9927 insertions(+)
create mode 100644 bootstrap/css/bootstrap-theme.css
create mode 100644 bootstrap/css/bootstrap-theme.min.css
create mode 100644 bootstrap/css/bootstrap.css
create mode 100644 bootstrap/css/bootstrap.min.css
create mode 100644 bootstrap/css/custom.css
create mode 100755 bootstrap/css/offcanvas.css
create mode 100644 bootstrap/fonts/glyphicons-halflings-regular.eot
create mode 100644 bootstrap/fonts/glyphicons-halflings-regular.svg
create mode 100644 bootstrap/fonts/glyphicons-halflings-regular.ttf
create mode 100644 bootstrap/fonts/glyphicons-halflings-regular.woff
create mode 100644 bootstrap/js/bootstrap.js
create mode 100644 bootstrap/js/bootstrap.min.js
create mode 100755 bootstrap/js/holder.js
create mode 100644 index.html
```

### 八、提交索引内容到仓库(repository)
 > git status

应该能看到类似如下的信息：
``` shell
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#   .project
#   bootstrap/
#   index.html
nothing added to commit but untracked files present (use "git add" to track) 
```

### 九、创建git账户及远程代码仓库

### 十、连接本地仓库与远程仓库
 > git remote add origin git@git.oschina.net:yeshaoting/martinye-demo-spring-velocity.git

应该能看到类似如下的信息：
``` shell
Counting objects: 20, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (20/20), done.
Writing objects: 100% (20/20), 121.70 KiB | 0 bytes/s, done.
Total 20 (delta 2), reused 0 (delta 0)
To https://github.com/your_name/your_repo.git
* [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```

### 十一、推送本地仓库到远程仓库
 > git push -u origin master

应该能看到类似如下的信息：
``` shell
Counting objects: 10, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (9/9), 1.33 KiB | 0 bytes/s, done.
Total 9 (delta 0), reused 0 (delta 0)
To git@git.oschina.net:yeshaoting/martinye-demo-spring-velocity.git
   8a82c26..90efea5  master -> master
分支 master 设置为跟踪来自 origin 的远程分支 master。
```

#### 补充说明
如若出现如下错误：
``` shell
To git@git.oschina.net:yeshaoting/martinye-demo-spring-velocity.git
 ! [rejected]        master -> master (fetch first)
error: 无法推送一些引用到 'git@git.oschina.net:yeshaoting/martinye-demo-spring-velocity.git'
提示：更新被拒绝，因为远程版本库包含您本地尚不存在的提交。这通常是因为另外
提示：一个版本库已向该引用进行了推送。再次推送前，您可能需要先整合远程变更
提示：（如 'git pull ...'）。
提示：详见 'git push --help' 中的 'Note about fast-forwards' 小节。
```

说明，远程代码仓库已经存在部分代码，且未 pull 最新代码到本地仓库，出现冲突。
此时，执行如下命令即可：
 > git pull origin master

``` shell
来自 git.oschina.net:yeshaoting/martinye-demo-spring-velocity
 * branch            master     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 LICENSE   | 191 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 README.md |   1 +
 2 files changed, 192 insertions(+)
 create mode 100644 LICENSE
 create mode 100644 README.md
```


[1]: https://www.texniq.de/en/web-engineering-i/create-local-git-repo-and-push-to-github