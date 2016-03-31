title: 生成javadoc文档
tags:
  - javadoc
  - java
categories:
  - 后端开发
date: 2016-03-11 18:33:00
---

<img src="/asserts/images/logo/java.png" class="img-logo img-center" />


## 一、查看javadoc帮助文档

``` bash
yerba-buena:~ yeshaoting$ javadoc --help
javadoc: 错误 - 无效的标记: --help
用法: javadoc [options] [packagenames] [sourcefiles] [@files]
  -overview <file>                 从 HTML 文件读取概览文档
  -public                          仅显示 public 类和成员
  -protected                       显示 protected/public 类和成员 (默认值)
  -package                         显示 package/protected/public 类和成员
  -private                         显示所有类和成员
  -help                            显示命令行选项并退出
  -doclet <class>                  通过替代 doclet 生成输出
  -docletpath <path>               指定查找 doclet 类文件的位置
  -sourcepath <pathlist>           指定查找源文件的位置
  -classpath <pathlist>            指定查找用户类文件的位置
  -cp <pathlist>                   指定查找用户类文件的位置
  -exclude <pkglist>               指定要排除的程序包列表
  -subpackages <subpkglist>        指定要递归加载的子程序包
  -breakiterator                   计算带有 BreakIterator 的第一个语句
  -bootclasspath <pathlist>        覆盖由引导类加载器所加载的
                                   类文件的位置
  -source <release>                提供与指定发行版的源兼容性
  -extdirs <dirlist>               覆盖所安装扩展的位置
  -verbose                         输出有关 Javadoc 正在执行的操作的信息
  -locale <name>                   要使用的区域设置, 例如 en_US 或 en_US_WIN
  -encoding <name>                 源文件编码名称
  -quiet                           不显示状态消息
  -J<flag>                         直接将 <flag> 传递到运行时系统
  -X                               输出非标准选项的提要

通过标准 doclet 提供:
  -d <directory>                   输出文件的目标目录
  -use                             创建类和程序包用法页面
  -version                         包含 @version 段
  -author                          包含 @author 段
  -docfilessubdirs                 递归复制文档文件子目录
  -splitindex                      将索引分为每个字母对应一个文件
  -windowtitle <text>              文档的浏览器窗口标题
  -doctitle <html-code>            包含概览页面的标题
  -header <html-code>              包含每个页面的页眉文本
  -footer <html-code>              包含每个页面的页脚文本
  -top    <html-code>              包含每个页面的顶部文本
  -bottom <html-code>              包含每个页面的底部文本
  -link <url>                      创建指向位于 <url> 的 javadoc 输出的链接
  -linkoffline <url> <url2>        利用位于 <url2> 的程序包列表链接至位于 <url> 的文档
  -excludedocfilessubdir <name1>:.. 排除具有给定名称的所有文档文件子目录。
  -group <name> <p1>:<p2>..        在概览页面中, 将指定的程序包分组
  -nocomment                       不生成说明和标记, 只生成声明。
  -nodeprecated                    不包含 @deprecated 信息
  -noqualifier <name1>:<name2>:... 输出中不包括指定限定符的列表。
  -nosince                         不包含 @since 信息
  -notimestamp                     不包含隐藏时间戳
  -nodeprecatedlist                不生成已过时的列表
  -notree                          不生成类分层结构
  -noindex                         不生成索引
  -nohelp                          不生成帮助链接
  -nonavbar                        不生成导航栏
  -serialwarn                      生成有关 @serial 标记的警告
  -tag <name>:<locations>:<header> 指定单个参数定制标记
  -taglet                          要注册的 Taglet 的全限定名称
  -tagletpath                      Taglet 的路径
  -charset <charset>               用于跨平台查看生成的文档的字符集。
  -helpfile <file>                 包含帮助链接所链接到的文件
  -linksource                      以 HTML 格式生成源文件
  -sourcetab <tab length>          指定源中每个制表符占据的空格数
  -keywords                        使程序包, 类和成员信息附带 HTML 元标记
  -stylesheetfile <path>           用于更改生成文档的样式的文件
  -docencoding <name>              指定输出的字符编码
```

<!-- more -->

## 二、命令举例

``` bash
# 输出所有cn.yeshaoting.jvwa包及子包
javadoc cn.yeshaoting.jvwa * -subpackages * -d ~/doc

# 输出所有cn.yeshaoting.jvwa下指定子包
javadoc cn.yeshaoting.jvwa * -subpackages cn.yeshaoting.jvwa.web cn.yeshaoting.jvwa.util -d ~/doc

# 输出所有cn.yeshaoting.jvwa下 package/protected/public 类和成员(默认值 -protected：只显示 protected/public 类和成员)
javadoc cn.yeshaoting.jvwa * -package -d ~/doc
```


## 三、自定义标签
自定义标签输出到javadoc文档中，可以通过参数-tag来完成。
javadoc帮助文档说明如下：
> `-tag <name>:<locations>:<header> 指定单个参数定制标记`

其中，name表示标签名称，locations表示处理标签所在的位置，header表示标签显示名。
另外，locations官方解释如下：
> Placement of tags - The Xaoptcmf part of the argument determines where in the source code the tag is allowed to be placed, and whether the tag can be disabled (using X). You can supply either a, to allow the tag in all places, or any combination of the other letters:
X (disable tag)
a (all)
o (overview)
p (packages)
t (types, that is classes and interfaces)
c (constructors)
m (methods)
f (fields)

一般地，直接使用a即可。


### 举例
``` bash
javadoc cn.yeshaoting.jvwa * -tag "api.name":a:"接口名：" -tag "api.apiId":a:"api id：" -tag "api.description":a:"接口描述：" -tag "api.category":a:"所属应用：" -tag "api.param":a:"参数：" -tag "api.result":a"返回结果：" -tag "api.requestExample":a:"请求实例：" -tag "api.responseExample":a:"响应实例：" -tag "api.errorExample":a:"错误实例：" -d ~/doc
```
**注**：程序中标签声明形如：@api.name，@api.description，@datetime。


## 四、生成chm文档
可参考：[Javadoc转换chm帮助文档的四种方法总结](http://www.blogjava.net/lishunli/archive/2010/01/07/308618.html#_Toc250550551)


## 五、参考文档
- [如何个性化地生成Javadoc文档](http://www.blogjava.net/lishunli/archive/2010/01/12/309218.html)
- [如何自动生成javadoc及添加自定义标签 ](http://blog.sina.com.cn/s/blog_6e371452010177dj.html)
- [多模块Maven项目如何使用javadoc插件生成文档](http://blog.csdn.net/jianxin1009/article/details/35269501)
- [Javadoc转换chm帮助文档的四种方法总结](http://www.blogjava.net/lishunli/archive/2010/01/07/308618.html#_Toc250550551)
