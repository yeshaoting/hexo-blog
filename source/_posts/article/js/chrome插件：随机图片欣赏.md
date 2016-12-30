title: chrome插件：随机图片欣赏
tags:
  - plugin
  - ''
  - chrome
categories:
  - 前端设计
author: yeshaoting
date: 2016-12-29 11:17:00
---

完整源码参见：https://github.com/yeshaoting/chrome-plugin-study/tree/master/start

## 一、插件文件

### 1. 清单文件：manifest.json

清单是chrome插件的入口，所有插件都要有这个文件。

包含插件的一些描述信息，例如：插件名称、版本号、允许请求的地址、展示的图标等。

**注：**文件名称必须为maifest.json

#### 内容

``` manifest
{
  "manifest_version": 2,

  "name": "图片欣赏",
  "description": "随机图片欣赏",
  "version": "1.0",

  "permissions": [
    "https://secure.flickr.com/"
  ],
  "browser_action": {
    "default_icon": "images/icon.png",
    "default_popup": "popup.html"
  }
}
```

### 2. 资源文件

清单文件添加的图标(images/icon.png)、插件点击后弹出页面(popup.html)等。

资源文件都是在清单中定义的，通过清单描述找到。

## 二、插件内页

### 1. 功能

本插件要实现的功能：

- 展示一批图片
- 批量更换所有图片
- 更换单个图片

### 2. 随机图片接口

现成的API接口：https://unsplash.it/300/300/?random=1

其中，random后面的值可以随机。

**注：**接口存在一个问题，在同一个页面random值相同的话，页面展示的图片是相同的。因此，要保证初始页面的所有random值不一致。

### 3. 点击事件

``` javascript
$(document).ready(function() {
  // 换一批
  $("div.head a.refresh").click(function(event) {
    var images = $("a.thumbnail img").each(function() {
      change_img($(this));
    });

  });

  // 单独更换
  $("a.thumbnail img").click(function(event) {
    change_img($(this));
  });

  function change_img($obj) {
    var url = "https://unsplash.it/300/300/?random=" + Math.random();
    $obj.attr("src", url);
  }

});
```

## 三、插件加载

进入chrome扩展程序，勾选右上方开发者模式，然后点击“加载已解压的扩展程序”。

在弹出的文件查看对话框，选择开发的插件在磁盘上的存放位置。

确认完成后，就能看到开发的插件“图片欣赏”，展示在下面的扩展程序列表里了。

在chrome地址栏右侧的扩展插件中，就能看到并使用刚开发的插件了。

![加载插件](http://7xkl4i.com1.z0.glb.clouddn.com/blog/Snip20161229_42.png)

## 四、效果图

### 1. 插件效果图

![插件效果图](http://7xkl4i.com1.z0.glb.clouddn.com/blog/Snip20161229_40.png)

### 2. 独立页面效果

![独立页面效果](http://7xkl4i.com1.z0.glb.clouddn.com/blog/Snip20161229_41.png)

## 五、参考文档

- [入门：建立 Chrome 扩展程序](https://crxdoc-zh.appspot.com/extensions/getstarted)
- [Chrome插件（Extensions）开发攻略](http://www.cnblogs.com/guogangj/p/3235703.html)