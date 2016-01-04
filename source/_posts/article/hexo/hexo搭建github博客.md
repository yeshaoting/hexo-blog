
title: 借助hexo搭建github博客
---

[TOC]

## 一、创建Github Pages
Github Pages为我们提供静态页面的托管。[^2]

### 1. 注册github账号
登录github.com，创建用户账号：yeshaoting。
访问地址：https://github.com/yeshaoting

### 2. 配置和使用github[^1]

#### 生成新的SSH Key
``` shell
ssh-keygen -t rsa -C "yeshaoting@163.com"
```
在~/.ssh目录会生成秘钥id_rsa，公钥id_rsa.pub

#### 添加SSH key到github
1) 查看并复制公钥id_rsa.pub内容。
2) 登录github，在Accout Settings -> SSH Public Keys -> add another public keys
3) 将复制id_rsa.pub内容复制到密钥文本框中。

#### 测试
``` shell
ssh -T git@github.com
```

<!-- more -->


### 3. 创建项目仓库
想建立个人博客，需要在github上建立yeshaoting.github.io的repository。
其中，repository名必须是 **github账号名**.github.io，我的github账号是yeshaoting，所以若想创建一个github pages博客，repository必须是yeshaoting.github.io；否则，创建不成功。

创建过程如下图所示：
![github上建立仓库](http://cnfeat.qiniudn.com/13.png)


## 二、安装hexo[^3]

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

官方中文网址：https://hexo.io/zh-cn/

### 1. 安装node js
登录：https://nodejs.org/，并下载安装包。
安装或解压node js到对应的目录 NODEJS_HOME。
将 $NODEJS_HOME/bin 目录添加到系统环境变量PATH里。

### 2. 安装hexo[^1]
Hexo的作者是tommy351，根据官方介绍，Hexo是一个简单、快速、强大的博客发布工具，支持Markdown格式。

``` shell
npm install -g hexo-cli
```

## 三、建站
创建hexo目录：hexo-blog


## 初始化并安装hexo
``` shell
cd hexo-blog
hexo init
npm install
```

新建完成后，指定文件夹的目录如下：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── scripts
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

目录结构说明如下：
**_config.yml**
网站的 配置 信息，您可以在此配置大部分的参数。

**package.json**
应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。

**scaffolds**
模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

**scripts**
脚本 文件夹。脚本是扩展 Hexo 最简易的方式，在此文件夹内的 JavaScript 文件会被自动执行。

**source**
资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

**themes**
主题 文件夹。Hexo 会根据主题来生成静态页面。


## 修改网站信息
打开_config.yml，修改网站信息
``` xml
# Site
title: 芳草地
subtitle: 叶绍亭
description:
author: yeshaoting
language: zh-CN
timezone: Asia/Shanghai
```


## 安装主题
官方收纳的主题：http://hexo.io/themes/
我选用 jacman 主题。
点击进入http://wuchong.me/jacman/介绍页有相关的安装指南[^4]。

**注**：下载完主题后，需要修改_config.yml文件，如下所示：
``` shell
theme: jacman
stylus:
  compress: true
```

## 四、服务部署[^3]
Hexo 提供了快速方便的一键部署功能，让您只需一条命令就能将网站部署到服务器上。

### 安装 hexo-deployer-git
`npm install hexo-deployer-git --save`

### 修改配置
``` shell
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:yeshaoting/yeshaoting.github.io.git
  branch: master
```

### 发布到github
hexo deploy

一般，之前需要生成静态文件hexo generate，
而在生成静态文件前，可以先清理之前的静态文件hexo clean。


## 五、本地预览

## 安装 hexo-server
`npm install hexo-server --save`

## 启动服务

### 默认端口为4000
hexo server

### 指定端口为5000
hexo server -p 5000

### 指定地址
hexo server -i 192.168.1.1


## 五、常用操作

### 创建文章
hexo new [layout] &lt;title>
新建的文章默认会被保存在source/_posts文件夹。
其中，layout默认为post，若指定，会在scaffolds文件夹找对应的布局文件。

### 常用命令
``` shell
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub
```

### 常用复合命令
``` shell
hexo d -g #生成加部署
hexo s -g #预览加部署
```

### 简写
``` shell
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```



[^1]: http://cnfeat.com/2014/05/10/2014-05-11-how-to-build-a-blog/
[^2]: http://www.pchou.info/web-build/2013/01/05/build-github-blog-page-02.html
[^3]: https://hexo.io/zh-cn/docs/
[^4]: http://wuchong.me/jacman/