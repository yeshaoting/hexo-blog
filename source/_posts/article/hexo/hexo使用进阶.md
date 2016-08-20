tags:
  - hexo
categories:
  - 博客搭建
title: hexo使用进阶
date: 2016-02-01 13:25:00
---

<img src="/asserts/images/logo/hexo.png" title="hexo" class="img-logo img-center" />


## 一、后端管理插件hexo-admin
插件可以直接在网页端创建、编辑markdown文章内容，并将内容发布到_posts里。
另外，对我而言，最方便的是可以很方便的给文章加标题、分类、打标签。

参见：
 - [An Admin Interface for Hexo](http://jaredforsyth.com/hexo-admin/)
 - [hexo-admin in github](https://github.com/jaredly/hexo-admin)

### 1. 安装
``` bash
npm install --save hexo-admin
hexo server -d
open http://localhost:4000/admin/
```

### 2. 配置
在_config.yml最后添加类似如下内容：
``` xml
admin:
  username: myfavoritename
  password_hash: be121740bf988b2225a313fa1f107ca1
  secret: a secret something
```

username：后端登录用户名
password_hash：后端登录用户密码对应的md5 hash值
secret：用于保证cookie安全

### 3. 预览
hexo-admin界面如下：
![screenshot-1](https://github.com/jaredly/hexo-admin/raw/master/docs/pasted-0.png?raw=true)
![screenshot-2](https://raw.githubusercontent.com/jaredly/hexo-admin/master/docs/pasted-1.png)

<!-- more -->


## 二、Error with DTrace
mac下默认安装使用hexo会报如下错误：
``` bash
{ [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
```

#### 解决办法
``` bash
npm install hexo --no-optional
```

参见：[Error with DTrace (Mac OS X)](https://hexo.io/docs/troubleshooting.html#Error_with_DTrace__28Mac_OS_X_29)


## 三、hexo不支持的语法
发现一例hexo不支持的行内代码语法：外层内容`${}`，内层内容以`#`开头。
但是支持多行代码块，如下：
``` bash
${#arr[@]}
${#arr[*]}
```


## 四、多分类支持不友好
如果文章A设置有二个分类如：linux, shell(有先后顺序)，另一篇文章B设置有分类如：shell, bash(有先后顺序)。
在分类里会生成四个分类linux(categories/linux), shell(categories/linux/shell), shell(categories/shell), bash(categories/shell/bash)。

由此可见，对于一篇文章最好只设置一个分类，设置多个标签。


## 五、文章更新时间显示
修改只对于jacman主题而言。

### 1. 添加文章更新时间显示位置
文件：layout/_partial/post/header.ejs
在article-time块内添加如下内容：
``` xml
    <time postupdated="<%= date_xml(item.updated) %>" itemprop="dateModified"> <%= __('dateModified') %> <%= item.updated.format(config.date_format) %></time>
```

### 2. 添加支持
文件：languages/zh-CN.yml
在最后添加如下内容：
> dateModified: 更新于

参见：
 - [hexo国际化（i18n）](https://hexo.io/zh-cn/docs/internationalization.html)
 - [hexo之jacman主题修改记录](https://blog.mcosx.cn/post/hexo-jacman/)

