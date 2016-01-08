categories:
  - linux
tags:
  - linux
  - 命令
  - 小记
title: Linux命令小记
date: 2016 2016-01-08 12:23:00
---

# Linux命令小记

[TOC]


## 1. cpu/内存占用率排序
``` shell
ps auxw --sort=%cpu
ps auxw --sort=%mem
```


## 2. 批量关闭进程
``` shell
kill -9 `ps -ef | grep QQ | grep -v 'grep' | awk {'print $2'}`
```


## 3. vim显示/隐藏行号
``` shell
:set nu 或 :set number
:set nonu 或 :set nonumber
```


## 4. 磁盘格式转换为ntfs( 会格式化磁盘 )
`convert X: /fs:ntfs`


## 5. 打印时间
``` shell
echo `date '+%Y-%m-%d %H:%M:%S'`
```

<!-- more -->

## 6. 批量解压zip文件
`for z in ./*.zip; do unzip -o $z -d ./peixun-logger ; done;`


## 7. 查找特定文件指定字符串
`grep "no object DCH for MIME type multipart/mixed" *.log > MIME.log`


## 8. 查看端口占用
``` shell
lsof -i:8080
ps -aux | grep java
ps -aux | grep pid
```


## 9. 进程批量中止
``` shell
kill -9 `ps -ef | grep QQ | grep -v grep | awk {'print $2'}`
```


## 10. 命令后台处理
`nohup command &`


## 11. 查看日历
cal


## 12. 时间显示
data

## 13. 查找文本内容
`grep -i -r --exclude=*.jar --exclude=*.log* noah .`


## 14. 查看tomcat日志特定内容
`tail -f catalina.out | grep timecost | grep FamilyController | awk '{if($10 > 100) print $0}'`


## 15. 查看tomcat某天日志
`tail -800000 catalina.out | grep 2014-06-30 > 2014-06-30.log`


## 16. 记录统计
``` shell
grep searchAddressForFamily 3.all.txt | wc -l
ls | wc -l
```


## 17. 查看文件或文件夹命令
linux 下查看文件个数及大小
`ls -l |grep "^-"|wc -l 或 find ./company -type f | wc -l`

查看某文件夹下文件的个数，包括子文件夹里的。
`ls -lR|grep "^-"|wc -l`

查看某文件夹下文件夹的个数，包括子文件夹里的。
`ls -lR|grep "^d"|wc -l`


## 18. 获取shell脚本位置/当前脚本目录
不能解决软链接问题：`$(cd "$(dirname "$0")"; pwd)`
最靠谱，能处理软链接的情况以及source加载脚本的情况：`echo $( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )`


## 19. tar打包文件，exclude选项排除指定文件或目录
`tar zcvf ../fetch-wiki.tar.gz * --exclude=*.txt* --exclude=logs`


## 20. 删除 - 开头的文件
``` shell
rm --  -filename
rm -- ./-filename
```


## 21. 查找特定字符串在文件中出现位置
`tail -2000000 catalina.out | cat -n | grep "2014-07-10" | head  -2 (对于出现在文末)`


## 22. 判断shell脚本输入参数个数
`if [ $# -eq 0 ]; then`


## 23. 逐行读取文本文件的 shell 脚本
``` shell
while read line
do
echo $line
done < $1
```

## 24. echo输出制表符方式
`echo -e "123\t345"`


## 25. for循环
``` shell
for index in {1..31}
do
    ……
done
```


## 26. 按修改先后，列出文件
`ls -lt`


## 27. 上条命令是否成功执行
``` shell
EXCODE=$?
if [ "$EXCODE" == "0" ]; then
echo "O.K"
else
echo "NOT O.K"
fi
```

## 28. 加载配置
``` shell
. slave.conf
source slave.conf
```

**注**：slave.conf里的值格式为K=V


## 29. 执行字符串命令
``` shell
SLAVE_PID_COMMAND="ps -ef | grep $SECRET_ID | grep -v grep | awk '{print \$2}'"
SLAVE_PID=$(eval $SLAVE_PID_COMMAND)
```

**注**：
\$要转义处理执行
执行字符串里的命令，使用eval命令
获取命令执行结果，可以用$()把命令包里来。


## 30. 去除空白行的sed
``` shell
SECRET=`sed '/^$/d' file.txt | head -1`
```


## 31. shell换行
`echo -e "123\n34"`

-e 表示特殊处理字符串里的特殊字符。


## 32. 获取某文件路径文件名
basename soft_install/jenkins-job-install/archive/jdk1.7.0_60.tar.gz .tar.gz => jdk1.7.0_60


## 33. 通过sed替换字特殊字符
`echo "12,34,56" | sed 's/,/ /g'`


## 34. 通过sed替换二者之间的字符串内容
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



## 35. vim替换指定区域匹配文本
```
: 41,57 s/10.13.92.143/10.13.92.157/g
```


## 36. 从替换开始匹配行到行尾或结束匹配行区间内替换内容
``` shell
sed -i "/ *server *id=/,\${s/\(\<watchdog-port>\).*\(<\/watchdog-port>\)/\18000\2/g;}" resin.conf
sed "/ *server *id=\"/,/<\/server>/{s/\(\<watchdog-port>\).*\(<\/watchdog-port>\)/\18888\2/g;}" resin.conf
```

**注**：每个匹配前后都需要加/，见上面红色部分。$表示最后一行，不需要加/


## 37. shell注释
``` shell
:<<comment
这里是个注释哈。
comment
```


## 38. 截取程序日志中某个时间范围内的文本
``` shell
sed -n '/^2015-05-14 15:32:30/,/^2015-05-14 16:12:09/p' access_log > log.txt
```


## 39. 重启ibus
``` shell
killall ibus-daemon
ibus-daemon -drx
```


## 40. 删除重复的、非连续的行
``` shell
`awk '! a[$0]++'`
```


## 41. awk使用特殊分隔符分隔列
``` shell
echo "192.168.102.134" | awk -F . '{print $4}'
```

## 42. 查看当前bash shell版本
echo $BASH_VERSION


## 43. 查看用户使用的shell版本
finger [USERNAME]

## 44. 递归查询不包含指定内容的文件
``` shell
grep -irL "date:" file.txt
```

