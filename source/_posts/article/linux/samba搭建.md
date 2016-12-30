title: samba搭建
tags:
  - samba
  - linux
categories:
  - 系统配置
author: yeshaoting
date: 2016-12-30 10:39:00
---

# 简介

Samba是在Linux和UNIX系统上实现SMB协议的一个免费软件，由服务器及客户端程序构成。SMB（Server Messages Block，信息服务块）是一种在局域网上共享文件和打印机的一种通信协议，它为局域网内的不同计算机之间提供文件及打印机等资源的共享服务.

# 安装

**注意：要安装samba必须有机器的root权限**

1. 下载samba-3.5.8.tar.gz到/home/work/xxx/(此处可以用自己指定path)
2. 解压tar zxvf samba-3.5.8.tar.gz
3. 执行 cd samba-3.5.8/source3
4. 执行 ./configure && make -j 4
5. root权限执行 make install

# 配置

1. 执行 cd /usr/local/samba/ (默认安装到该路径下)

2. 执行 vi lib/smb.conf (将下面的内容粘贴到smb.conf)

   > [global]
   > diplay charset = utf8
   > unix charset = gbk
   > dos charset = gbk
   > workgroup = work
   > netbios name = work
   > server string = uc
   > security = user
   > [work]
   > comment = uc
   > path=/home/work
   > create mask = 0664
   > directory mask = 0775
   > writable = yes
   > valid users = work
   > browseable = yes

3. 配置项说明

   > [global] 标识全局配置： 前三行是对编码的配置。
   > workgroup：设置服务器所要加入的工作组的名称，会在Windows 的“网上邻居”中能看到work工作组，可以在此设置所需要的工作组的名称。
   > netbios name：设置出现在“网上邻居”中的主机名。默认情况下，则使用真正的主机名。
   > server string：设置服务器主机的说明信息，当在Windows 的“网上邻居”中打开Samba 上设置的工作组时，在Windows 的资源管理器窗口，会列出“名称”和“备注”栏，其中“名称”栏会显示出Samba服务器的NetBios名称，而“备注”栏则显示出此处设置的“Samba Server”。当然，可以修改默认的“Sambe Server”，使用自己的描述信息。
   > security：设置Samba服务器的安全等级。默认情况下，使用user等级。
   > Samba服务器一共有四种安全等级：
   > （1）share:使用此等级，用户不需要帐号及密码可以登陆Samba服务器。
   > （2）user:使用此等级，由提供服务的Samba服务器检查用户帐号及密码。
   > （3）server:使用此等级，检查帐号及密码的工作可指定另一台Samba服务器负责。
   > （4）domain:使用此等级，需要指定一台Windows NT/2000/XP服务器（通常为域控制器），以验证用户输入的帐号及密码。
   > [work]标识Samba文件共享配置（中括号中名称任意）
   > comment: 针对共享资源所作的说明、注释部分。
   > path: 若共享资源是目录，则指定目录的位置；若为打印机，则指定打印机队列的位置。
   > create mask: 设置文件的访问权限，默认值为0744。
   > directory mask: 设置目录的访问权限，默认值为0755。
   > writeable: 设置共享的资源是否可以写入。若共享资源是打印机，则不需设置此参数
   > valid users: 设置合法用户名。
   > browseable: 设置用户是否可以看到此共享资源。默认值为yes，若将此参数设置为no，用户虽然看不到此资源，但是拥有权限的用户仍可直接输入该资源的网址来访问该资源

# 启动和使用samba

1. 执行 vi ~/.bash_profile(在文件里添加如下内容), 保存退出

   > export LD_LIBRARY_PATH=/usr/local/samba/lib:$LD_LIBRARY_PATH

2. 执行 . ~/.bash_profile

3. 执行 ./bin/smbpasswd -a work (设置work的密码)

4. 执行 cd /usr/local/samba/sbin

5. 执行 ./smbd -D (启动samba)

6. ps auxf | grep smbd 查看进程是否启动; netstat –npl 查看samba端口号，默认会使用139、445两个端口号