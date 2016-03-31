title: jacman主题开启jiathis分享
tags:
  - hexo
  - 分享代码
categories:
  - 博客搭建
date: 2016-02-16 15:45:00
---

<img src="/asserts/images/logo/hexo.png" class="img-logo img-center" />


## 一、前言
本文内容基于wuchong的Jacman主题。

### 1. 默认分享样式
默认分享样式如下图所示：
![默认分享](http://7xkl4i.com1.z0.glb.clouddn.com/hexo-jacman-default-share.png)

### 2. 代码位置
分享按钮位置：themes/jacman/layout/_partial/post/footer.ejs
代码块：
``` html
<div class="article-share" id="share">
</div>
```


## 二、开启jiathis分享
jacman主题默认支持jiathis分享，只需要如下简单几步工作即可开启：

### 1. 注册jiathis
登录[jiathis网站](http://www.jiathis.com/)，完成注册。
进入账号设置，在基本资料栏可以获取用户ID：2084038。

### 2. 主题配置
修改主题配置文件themes/jacman/_config.yml，如下：

``` txt
#### Share button
jiathis:
  enable: true ## if you use jiathis as your share tool,the built-in share tool won't be display.
  id: 2084038   ## e.g. 1889330 your jiathis ID.
  tsina: 1873363984 ## e.g. 2176287895 Your weibo id,It will be used in share button.
```

**注**：enable, id, tsina前有二个空格(表示层级关系)，在代码里，可能通过theme.jiathis.enable获取其值。

### 3. 代码逻辑
代码：themes/jacman/layout/_partial/post/footer.ejs

jiathis分享相关代码逻辑：
``` html
<% if(theme.jiathis.enable){ %>
<div class="share-jiathis">
  <%- partial('jiathis') %>
 </div>
<% } else { %>
  <div data-url="<%- item.permalink %>" data-title="<% if (item.title){ %><%= item.title %> | <% } %><%= config.title %>" data-tsina="<%= theme.author.tsina %>" class="share clearfix">
  </div>
<% } %>
```

**注**：`<%- partial('jiathis') %>`语法表示加载当前目录下的jiathis.ejs文件。

### 4. 调整分享按钮顺序
代码：themes/jacman/layout/_partial/post/jiathis.ejs

调整后的顺序：
``` html
<div class="jiathis_style_24x24">
    <a class="jiathis_button_cqq"></a>
    <a class="jiathis_button_qzone"></a>
    <a class="jiathis_button_weixin"></a>
    <a class="jiathis_button_tsina"></a>
    <a class="jiathis_button_renren"></a>
    <a href="http://www.jiathis.com/share" class="jiathis jiathis_txt jtico jtico_jiathis" target="_blank"></a>
    <a class="jiathis_counter_style"></a>
</div>
```


## 三、分享效果展示
分享按钮样式：
![jiathis分享](http://7xkl4i.com1.z0.glb.clouddn.com/hexo-jacman-jiathis-share.png)

内容分享给QQ好友：
![分享给QQ好友](http://7xkl4i.com1.z0.glb.clouddn.com/hexo-jacman-jiathis-share-qq.png)

内容分享到微博：
![分享到微博](http://7xkl4i.com1.z0.glb.clouddn.com/hexo-jacman-jiathis-share-weibo.png)


## 四、小结

### 1. 分享内容丰富
jiathis分享最大的一个优点在于主流的社交平台分享的内容更加丰富：描述会带上一部分正方内容，另外会附带上正文中的图片。

### 2. 累计分享次数统计
较多说分享而言，jiathis多出一个累计分享次数统计功能。
较百度分享而言，找了好久才在别人的博客里找到百度分享累计分享次数的代码，未在百度分享网站显目位置有说明介绍。
