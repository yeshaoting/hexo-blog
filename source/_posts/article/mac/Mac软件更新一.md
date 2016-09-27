title: Mac装机软件
tags:
    - 小记
    - mac
    - 装机
categories:
    - 工具软件

date: 2015-12-29 17:56:00
---

![Mac icon](http://o7kubqw1j.bkt.clouddn.com/asserts/images/logo/mac.png)

<img src="http://o7kubqw1j.bkt.clouddn.com/asserts/images/logo/mac.png" class="img-logo img-center" />



## 一、应用软件





## 二、命令工具



### 1. cloc

cloc(Count Lines of Code)，一款代码行统计工具。

### 命令安装

``` shell
npm install -g cloc                    # https://www.npmjs.com/package/cloc
sudo apt-get install cloc              # Debian, Ubuntu
sudo yum install cloc                  # Red Hat, Fedora
sudo dnf install cloc                  # Fedora 22 or later
sudo pacman -S cloc                    # Arch
sudo pkg install cloc                  # FreeBSD
sudo port install cloc                 # Mac OS X with MacPorts
brew install cloc                      # Mac OS X with Homebrew
choco install cloc                     # Windows with Chocolatey
```

#### 效果展示

顺便统计了某个项目目录下的代码行，效果如下：

``` shell
yerba-buena:java yeshaoting$ cloc ./
     205 text files.
     163 unique files.
      69 files ignored.

github.com/AlDanial/cloc v 1.70  T=1.05 s (130.4 files/s, 7699.2 lines/s)
----------------------------------------------------------------------------------------
Language                              files          blank        comment           code
----------------------------------------------------------------------------------------
XML                                     105             87            521           5983
Maven                                     1             35              9            357
JavaScript                                1             29             15            332
Java                                      6             63              8            199
HTML                                      6              6              0            105
Markdown                                  1             35              0             88
Freemarker Template                       2              2              0             41
Velocity Template Language                2              2              0             38
JSP                                       3              0              0             37
Mustache                                  4              0              0             32
Twig                                      3              1              0             31
Handlebars                                2              0              0             22
YAML                                      1              0              0              8
----------------------------------------------------------------------------------------
SUM:                                    137            260            553           7273
----------------------------------------------------------------------------------------
```

参见：https://github.com/AlDanial/cloc

### 2. tree

命令行输出文件树形结构

#### 安装

``` she
brew install tree
```

#### 效果

``` shell
yerba-buena:examples yeshaoting$ tree
.
├── InMemoryPresentationsRepository.java
├── PresentationsRepository.java
├── controller
│   └── PresentationsController.java
├── model
│   └── Presentation.java
└── services
    └── PresentationsService.java
```

### 3. wget

### 4. sqlmap

### 5. mycli

### 6. cheat

### 7. innotop

### 8. Mac OSX Copy and Paste Commands

| Command | Function             |
| ------- | -------------------- |
| pbcopy  | Copy to Clipboard    |
| pbpaste | Paste from Clipboard |

参见：[Copy to and Paste From the Clipboard on the Mac OSX Command Line](http://sweetme.at/2013/11/17/copy-to-and-paste-from-the-clipboard-on-the-mac-osx-command-line/)

### 9. SHC

Shell script compiler

参见：https://neurobin.org/projects/softwares/unix/shc/

### 10. Oraji

参见：https://neurobin.org/projects/softwares/unix/oraji/

### 11. qshell

https://github.com/qiniu/qshell

http://developer.qiniu.com/code/v6/tool/qshell.html

## 三、下载网站

brew命令下载：http://brewformulas.org/