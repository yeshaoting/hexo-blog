title: yelee主题最近访客挂件
tags:
  - hexo
categories:
  - 博客搭建
author: yeshaoting
date: 2016-08-20 14:28:00
---

<img src="/asserts/images/logo/blog9.png" title="blog" class="img-logo img-center" />

## 一、前言
最近将博客hexo主题由原来的jacman换成高大上的yelee。
原来在jacman主题自定义了一些挂件，例如:多说的最近访客。
希望将原来的 **最近访客** 挂件迁移到yelee主题。


## 二、菜单项

### 1. 查找菜单项文件
通过chrome开发者工具，查看菜单 **关于我** 菜单项内容元素。
``` html
<section class="switch-part switch-part4">
    <div id="js-aboutme">只求成为一名靠谱的码农</div>
</section>
```

然后，根据元素标签特征(这里选 **switch-part4** )，在yelee主题目录搜索layout目录下包含 **switch-part4** 的文件内容。
``` bash
yerba-buena:yelee yeshaoting$ grep -ir "switch-part4" layout/
layout//_partial/left-col.ejs:                <section class="switch-part switch-part4">
```

可以看到，该文件内容存在于 layout//_partial/left-col.ejs 目录。

### 2. 添加菜单项
在left-col.ejs文件里，找到tips-box元素块，并类似aboutme添加visitor，如下:
``` html
<div class="tips-box hide">
    <div class="tips-arrow"></div>
    <ul class="tips-inner">
        <li><%= __('index.menu') %></li>
        <li><%= __('index.tags') %></li>
        <%if(theme.friends && theme.friends.length != 0){%>
        <li><%= __('index.friends') %></li>
        <%}%>
        <%if(theme.aboutme){%>
        <li><%= __('index.about') %></li>
        <%}%>
        <%if(theme.visitor){%>
        <li><%= __('index.visitor') %></li>
        <%}%>
    </ul>
</div>
```

### 3. 菜单项名称
由于主题支持多语言，因此通过 `__('index.visitor')` 这些的方法从语言包里获取对应的内容。
该语言文件在主题的languages目录。我的设置为:
``` plain
# zh-Hans: Chinese (Simplified) 大陆简体
index:
  menu: 菜单
  tags: 标签
  friends: 友情链接
  about: 个人签名
  visitor: 最近访客
  more: 阅读全文
  copy: 复制
```

### 4. 开启菜单项
注意到主题配置文件里，通过aboutme来开关“关于我”菜单项。
同样，我们也在配置文件里添加一项来控制“最近访客”菜单项是否展示。如下:
``` bash
#是否开启“最近访客”功能。
visitor: true
```

### 5. 效果图
![最近访客菜单项](/asserts/images/article/yelee主题最近访客/2016-08-20 13-41-17.jpg)


<!-- more -->

## 三、最近访客详情

### 1. 准备挂件
如果主题开启并使用了多说评论的话，只需要添加如下内容即可:
``` html
<ul class="ds-recent-visitors"></ul>
```

如果并未开启多说评论的话，需要注册多说并额外添加多说js内容如下:
``` html
<!--多说js加载开始，一个页面只需要加载一次 -->
<script type="text/javascript">
var duoshuoQuery = {short_name:"您的多说二级域名"};
(function() {
    var ds = document.createElement('script');
    ds.type = 'text/javascript';ds.async = true;
    ds.src = 'http://static.duoshuo.com/embed.js';
    ds.charset = 'UTF-8';
    (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(ds);
})();
</script>
<!--多说js加载结束，一个页面只需要加载一次 -->
```

这部分内容为了更加通用，可以将内容写入到ejs模板文件中，然后通过加载外部模板方式引入到主题中。
我就是将上述内容放在自定义模板文件: `layout/_plugin/duoshuo_visitor.ejs` 。

参见:[通用代码【最近访客】使用方法](http://dev.duoshuo.com/docs/4ff28d6f552860f21f00000c)

### 2. 添加多说最近访客
``` html
<%if(theme.visitor){%>
<section class="switch-part switch-part5">
    <div class="widget visitor" id="js-visitor">
       <%- partial('_plugin/duoshuo_visitor') %>
    </div>
</section>
<%}%>
```

### 3. 最近访客样式
打开主题下 source/css/_partial/main.styl 文件。
找到switch-part4节点，然后类似的添加 switch-part5 内容，如下:
``` css
.switch-part4{
    left: 300%;
    text-align: left;
    line-height: 30px;
}
.switch-part5{
    left: 400%;
    line-height: 30px;
    max-height: 260px;
}
```

### 4. 添加详情页菜单项图标
直接copy aboutme的图标内容。
``` html
<%if(theme.visitor){%>
<div class="icon-wrap icon-me hide" data-idx="4">
    <div class="user"></div>
    <div class="shoulder"></div>
</div>
<%}%>
```

### 5. 效果图
![最近访客详情页](/asserts/images/article/yelee主题最近访客/2016-08-20 14-27-49.jpg)

