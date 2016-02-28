categories:
  - linux
tags:
  - 命令
  - 小记
title: Linux命令小记
date: 2016-01-15 18:02:00
---

## 一、awk命令

### 1. 删除重复的、非连续的行
``` shell
`awk '! a[$0]++'`
```


### 2. awk使用特殊分隔符分隔列
``` shell
echo "192.168.102.134" | awk -F . '{print $4}'
```


## 二、sed命令

### 1. 截取程序日志中某个时间范围内的文本
``` shell
sed -n '/^2015-05-14 15:32:30/,/^2015-05-14 16:12:09/p' access_log > log.txt
```

### 2. 去除空白行的sed
``` shell
SECRET=`sed '/^$/d' file.txt | head -1`
```

### 3. 通过sed替换字特殊字符
`echo "12,34,56" | sed 's/,/ /g'`


### 4. 通过sed替换二者之间的字符串内容
文本内容：
``` xml
<server   id="film-web"   address="127.0.0.1" port="6881">
</server>
```

``` shell
sed -n "s/\( *server.*port=\"\).*\(\".*\)/\1abc\2/p" 1.txt
sed -n "s/\( *server.*address=\"\).*\(\" *port\)/\1abc\2/p" 1.txt
```

**注**：sed里，使用圆括号括起来的内容，可以看做是变量。使用\1和\2来输出。
**参见**：http://coolshell.cn/articles/9104.html 圆括号匹配


### 5. 从替换开始匹配行到行尾或结束匹配行区间内替换内容
``` shell
sed -i "/ *server *id=/,\${s/\(\<watchdog-port>\).*\(<\/watchdog-port>\)/\18000\2/g;}" resin.conf
sed "/ *server *id=\"/,/<\/server>/{s/\(\<watchdog-port>\).*\(<\/watchdog-port>\)/\18888\2/g;}" resin.conf
```

**注**：每个匹配前后都需要加/，见上面红色部分。$表示最后一行，不需要加/


## 三、grep命令

### 1. 查找特定文件指定字符串
`grep "no object DCH for MIME type multipart/mixed" *.log > MIME.log`


### 2. 查找文本内容
`grep -i -r --exclude=*.jar --exclude=*.log* noah .`


### 3. 查看tomcat日志特定内容
`tail -f catalina.out | grep timecost | grep FamilyController | awk '{if($10 > 100) print $0}'`


### 4. 查看tomcat某天日志
`tail -800000 catalina.out | grep 2014-06-30 > 2014-06-30.log`


### 5. 记录统计
``` shell
grep timecost file.txt | wc -l
ls | wc -l
```

### 6. grep与或非

#### 与操作
``` shell
grep -E 'pattern1.*pattern2' file.txt # in that order
grep -E 'pattern1.*pattern2|pattern2.*pattern1' file.txt # in any order
grep 'pattern1' file.txt | grep 'pattern2' # in any order
```

#### 或操作
``` shell
grep "pattern1\|pattern2" file.txt
grep -E "pattern1|pattern2" file.txt
grep -e pattern1 -e pattern2 file.txt
egrep "pattern1|pattern2" file.txt
```

#### 非操作
``` bash
grep -v 'pattern1' file.txt
```

摘自：[Using BASH "Grep OR / Grep AND / Grep NOT" Operators](http://www.shellhacks.com/en/Using-BASH-Grep-OR-Grep-AND-Grep-NOT-Operators)


### 7. 递归查询不包含指定内容的文件
``` shell
grep -irL "date:" file.txt
```


<!-- more -->


## 四、ps命令

### 1. cpu/内存占用率排序
``` bash
ps auxw --sort=%cpu
ps auxw --sort=%mem
```

### 2. 查看端口占用
``` bash
lsof -i:8080
ps -aux | grep java
ps -aux | grep pid
```

## 五、kill命令

### 1. 批量关闭进程
``` bash
kill -9 `ps -ef | grep QQ | grep -v 'grep' | awk {'print $2'}`
```

### 2. 进程批量中止
``` bash
kill -9 `ps -ef | grep QQ | grep -v grep | awk {'print $2'}`
```

## 六、vim命令

### 1. vim显示/隐藏行号
``` vim
:set nu 或 :set number
:set nonu 或 :set nonumber
```

### 2. vim替换指定区域匹配文本
``` vim
: 41,57 s/10.13.92.143/10.13.92.157/g
```

## 七、date命令

### 1. 打印时间
``` bash
echo `date '+%Y-%m-%d %H:%M:%S'`
```

### 2. 时间显示
``` bash
date
date '+%Y-%m-%d %H:%M:%S'
```

## 八、cal命令

### 1. 当年当月
`cal`

### 2. 某年日历
`cal 2015`

### 3. 某年某月
`cal 12 2015`


## 九、ls命令

### 1. linux 下查看文件个数及大小
`ls -l |grep "^-"|wc -l 或 find ./company -type f | wc -l`

### 2. 查看某文件夹下文件的个数，包括子文件夹里的。
`ls -lR|grep "^-"|wc -l`

### 3. 查看某文件夹下文件夹的个数，包括子文件夹里的。
`ls -lR|grep "^d"|wc -l`

### 4. 按修改先后，列出文件
`ls -lt`


## 十、tar命令

### 1. tar打包文件，exclude选项排除指定文件或目录
`tar zcvf ../fetch-wiki.tar.gz * --exclude=*.txt* --exclude=logs`


## 十一、rm命令

### 1. 删除 - 开头的文件
``` shell
rm --  -filename
rm -- ./-filename
```


## 十二、echo命令

### 1. echo输出制表符方式
`echo -e "123\t345"`


### 2. shell换行
``` bash
echo -e "123\n34"
```

**注**：-e 表示特殊处理字符串里的特殊字符。


## 十三、tail语法

### 1. 查找特定字符串在文件中出现位置
`tail -2000000 catalina.out | cat -n | grep "2014-07-10" | head  -2 (对于出现在文末)`


## 十四、shell语法

### 1. 批量解压zip文件
`for z in ./*.zip; do unzip -o $z -d ./peixun-logger ; done;`

### 2. 判断shell脚本输入参数个数
`if [ $# -eq 0 ]; then`


### 3. 逐行读取文本文件的 shell 脚本
``` shell
while read line
do
echo $line
done < $1
```

### 4. for循环
``` shell
for index in {1..31}
do
    ……
done
```

### 5. shell注释
``` shell
:&lt;&lt;comment
这里是个注释哈。
comment
```

### 6. 获取shell脚本位置/当前脚本目录
不能解决软链接问题：`$(cd "$(dirname "$0")"; pwd)`
最靠谱，能处理软链接的情况以及source加载脚本的情况：`echo $( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )`


### 7. 查看当前bash shell版本
echo $BASH_VERSION


### 8. 理解 bashrc 和 profile
参见：[理解 bashrc 和 profile](https://wido.me/sunteya/understand-bashrc-and-profile)

#### login shell 和 no-login shell
“login shell” 代表用户登入, 比如使用 “su -“ 命令, 或者用 ssh 连接到某一个服务器上, 都会使用该用户默认 shell 启动 login shell 模式.
该模式下的 shell 会去自动执行 /etc/profile 和 ~/.profile 文件, 但不会执行任何的 bashrc 文件, 所以一般再 /etc/profile 或者 ~/.profile 里我们会手动去 source bashrc 文件.
而 no-login shell 的情况是我们在终端下直接输入 bash 或者 bash -c “CMD” 来启动的 shell.
该模式下是不会自动去运行任何的 profile 文件.

#### interactive shell 和 non-interactive shell
interactive shell 是交互式shell, 顾名思义就是用来和用户交互的, 提供了命令提示符可以输入命令.
该模式下会存在一个叫 PS1 的环境变量, 如果还不是 login shell 的则会去 source /etc/bash.bashrc 和 ~/.bashrc 文件
non-interactive shell 则一般是通过 bash -c “CMD” 来执行的bash.
该模式下不会执行任何的 rc 文件, 不过还存在一种特殊情况这个我之后详细讲述

#### bashrc 和 profile 的区别
看了之前那么多种状态组合, 最关键的问题是, 究竟 bashrc 和 profile 有什么区别呢?

**profile**
其实看名字就能了解大概了, profile 是某个用户唯一的用来设置环境变量的地方, 因为用户可以有多个 shell 比如 bash, sh, zsh 之类的, 但像环境变量这种其实只需要在统一的一个地方初始化就可以了, 而这就是 profile.

**bashrc**
bashrc 也是看名字就知道, 是专门用来给 bash 做初始化的比如用来初始化 bash 的设置, bash 的代码补全, bash 的别名, bash 的颜色. 以此类推也就还会有 shrc, zshrc 这样的文件存在了, 只是 bash 太常用了而已.


### 9. 上条命令是否成功执行
``` bash
EXCODE=$?
if [ "$EXCODE" == "0" ]; then
echo "O.K"
else
echo "NOT O.K"
fi
```

### 10. 加载配置
``` shell
. slave.conf
source slave.conf
```

**注**：slave.conf里的值格式为K=V

### 11. 执行字符串命令
``` shell
SLAVE_PID_COMMAND="ps -ef | grep $SECRET_ID | grep -v grep | awk '{print \$2}'"
SLAVE_PID=$(eval $SLAVE_PID_COMMAND)
```

**注**：
\$要转义处理执行
执行字符串里的命令，使用eval命令
获取命令执行结果，可以用$()把命令包里来。


### 12. 获取某文件路径文件名
basename soft_install/jenkins-job-install/archive/jdk1.7.0_60.tar.gz .tar.gz => jdk1.7.0_60


## 十五、find命令

### 1. 删除所有文件名包含Ulysses-Group的文件
`find . -name "*Ulysses-Group*" -exec rm -rf {} \;`


## 十六、其他命令

### 1. 磁盘格式转换为ntfs( 会格式化磁盘 )
`convert X: /fs:ntfs`

### 2. 命令后台处理
`nohup command &`

### 3. 重启ibus
``` shell
killall ibus-daemon
ibus-daemon -drx
```

### 4. 查看用户使用的shell版本
finger [USERNAME]

### 5. tee
重定向到文件并打印到屏幕

参见：[tee--重定向到文件并打印到屏幕](http://blog.csdn.net/love_gaohz/article/details/8100808)


