title: jacman主题集成百度分享
tags:
  - 百度分享
  - 分享代码
categories:
  - hexo
date: 2016-02-16 16:53:00
---

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


## 二、集成百度分享
jacman主题默认不支持百度分享，只需要如下简单几步工作即可集成：

### 1. 登录或注册百度
登录[百度分离](http://share.baidu.com/)，完成登录或注册。

### 2. 主题配置
修改主题配置文件themes/jacman/_config.yml，如下：

``` txt
bdshare:
  enable: true ## if you use baidu share as your share tool,the built-in share tool won't be display.
```

**注**：enable前有二个空格(表示层级关系)，在代码里，可能通过theme.bdshare.enable获取其值。

### 3. 代码逻辑
代码：themes/jacman/layout/_partial/post/footer.ejs

百度分享相关代码逻辑：
``` html
<div class="article-share" id="share">
    <% if(theme.bdshare.enable){ %>
      <%- partial('bdshare') %>
    <% } else { %>
        <% if(theme.jiathis.enable){ %>
        <div class="share-jiathis">
          <%- partial('jiathis') %>
         </div>
        <% } else { %>
          <div data-url="<%- item.permalink %>" data-title="<% if (item.title){ %><%= item.title %> | <% } %><%= config.title %>" data-tsina="<%= theme.author.tsina %>" class="share clearfix">
          </div>
        <% } %>
    <% } %>
    </div>
```

在原来theme.jiathis.enable代码块外层包一层theme.bdshare.enable判断，优先判断是否使用百度分享。

**注**：`<%- partial('bdshare') %>`语法表示加载当前目录下的bdshare.ejs文件。

### 4. 百度分享模块
代码：themes/jacman/layout/_partial/post/bdshare.ejs

新建bdshare.ejs，并复制如下内容：
``` html
<% if (theme.bdshare.enable){ %>
    <div class="bdsharebuttonbox">
        <a href="#" class="bds_more" data-cmd="more"></a>
        <a href="#" class="bds_sqq" data-cmd="sqq" title="分享到QQ好友"></a>
        <a href="#" class="bds_qzone" data-cmd="qzone" title="分享到QQ空间"></a>
        <a href="#" class="bds_weixin" data-cmd="weixin" title="分享到微信"></a>
        <a href="#" class="bds_tsina" data-cmd="tsina" title="分享到新浪微博"></a>
        <a href="#" class="bds_renren" data-cmd="renren" title="分享到人人网"></a>
        <a href="#" class="bds_count" data-cmd="count"></a>
    </div>
    <script>window._bd_share_config={"common":{"bdSnsKey":{},"bdText":"","bdMini":"1","bdMiniList":false,"bdPic":"","bdStyle":"0","bdSize":"24"},"share":{}};with(document)0[(getElementsByTagName('head')[0]||body).appendChild(createElement('script')).src='http://bdimg.share.baidu.com/static/api/js/share.js?v=89860593.js?cdnversion='+~(-new Date()/36e5)];</script>
<% } %>
```

### 5. 检测分享代码版本
``` javascript
//打开已安装分享代码的页面，在控制台中执行：
javascript:b=(window.bdShare||window._bd_share_main);alert(b?'\u5F53\u524D\u9875\u9762\u7684\u5206\u4EAB\u4EE3\u7801\u7248\u672C\u4E3A\uFF1A'+(b.version||'1.0'):'\u5F53\u524D\u9875\u9762\u6CA1\u6709\u5B89\u88C5\u5206\u4EAB\u4EE3\u7801\u3002')
```

参见：[检测分享代码版本](http://share.baidu.com/code/advance#tools)


## 三、分享效果展示
分享按钮样式：
![jiathis分享](http://7xkl4i.com1.z0.glb.clouddn.com/hexo-jacman-bdshare-share.png)

内容分享给QQ好友：
![分享给QQ好友](http://7xkl4i.com1.z0.glb.clouddn.com/hexo-jacman-bdshare-share-qq.png)

内容分享到微博：
![分享到微博](http://7xkl4i.com1.z0.glb.clouddn.com/hexo-jacman-bdshare-share-weibo.png)


## 四、小结
百度分享最大的一个优点在于更能提高在百度的搜索排名。另外，个人感觉百度分享生成代码过程更加友好。
内容分享描述比较简单，仅仅只支持标题是硬伤。
