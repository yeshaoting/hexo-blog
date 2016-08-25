tags:
- prism

categories:
- 前端设计

author: yeshaoting
title: 代码高亮类库prismjs
date: 2016-08-25 15:45:00
---
<img src="/asserts/images/logo/prismjs.jpg" class="img-logo img-center" />


## 一、简介

Prism 是一款轻量、可扩展的代码语法高亮库，使用现代化的 Web 标准构建。



## 二、为什么选择 Prism.js ？

>-  极致易用
>   引用 prism.css 和 prism.js，使用合适的 HTML5 标签（code.language-xxxx），搞定！
>
>-  天生伶俐
>    语言的 CSS 类是可继承的，所以你只需定义一次就能应用到多个代码片段。
>
>-  轻如鸿毛
>    代码压缩后只有 1.6KB。每添加一个语言平均增加 0.3-0.5KB，主题在 1KB 左右。
>
>-  快如闪电
>     如果可能，支持通过 Web Workers 实现并行。
>
>-  轻松扩展
>    定义新语言或扩展现有语法，或者新增功能都非常简单。
>
>-  丰富样式
>    所有的样式通过 CSS 完成，并使用合理的类名如：.comment, .string, .property 等。



## 三、简单用法



### 1. head添加主题样式

``` html
<link rel="stylesheet" href="http://prismjs.com/themes/prism.css">
```



### 2. 文档末尾添加Prism 类库

``` html
<script src="http://prismjs.com/prism.js"></script>
```



### 3. 添加测试代码 

> 遵循 [HTML5 标准](http://www.w3.org/TR/html5/grouping-content.html#the-pre-element)，Prism 使用语义化的 `pre` 元素和 `code` 元素来标记代码区块。

``` html
<pre class="language-css"><code>p { color: red }</code></pre>
```



### 4. 完整代码

``` html
<!DOCTYPE html>
<html>
<head>
	<title>代码高亮类库Prism.js</title>
	<link rel="stylesheet" href="http://prismjs.com/themes/prism.css">
</head>
<body>
	<pre class="language-css"><code>p { color: red }</code></pre>
	<script src="http://prismjs.com/prism.js"></script>
</body>
</html>
```



### 5. 输出样式

![Prism高亮效果](http://o7kubqw1j.bkt.clouddn.com/asserts/images/article/代码高亮类库Prism/Snip20160825_53.png)

<!-- more -->



## 四、扩展内容



### 1. 高亮语法支持

默认的prism-core只支持少量的语法高亮，如：html, css, javascript等。

如需要针对java、C、python语法进行高亮显示的话，需要额外引入特定语法高亮类库。

``` html
<script src="http://prismjs.com/components/prism-java.js"></script>
<script src="http://prismjs.com/components/prism-sql.js"></script>
<script src="http://prismjs.com/components/prism-python.js"></script>
```

**注**：扩展的语法高亮类库位置放于prism.js之后。另外，也可以在下载prismjs里把需要的类库都打包进prismjs.js里。



效果如下图所示：

![java语法高亮效果](http://o7kubqw1j.bkt.clouddn.com/asserts/images/article/代码高亮类库Prism/Snip20160825_54.png)



### 2. 高亮主题  

prismjs支持多种高亮主题，默认的就是上述截图的主题。

如果有其他的主题偏好，可以更换prismjs主题样式。

方法如下 ：

``` html
<link rel="stylesheet" href="http://prismjs.com/themes/prism-okaidia.css">
<link rel="stylesheet" href="http://prismjs.com/themes/prism-solarizedlight.css">
<link rel="stylesheet" href="http://prismjs.com/themes/prism-dark.css">
<link rel="stylesheet" href="http://prismjs.com/themes/prism-twilight.css">
<link rel="stylesheet" href="http://prismjs.com/themes/prism-coy.css">
```

除了本文列出的几种主题外，还支持其他几种，不一一列举。

就列出的这几种主题而言，个人比较喜欢 **亮色的coy** 主题和  **暗色的dark** 主题。



## 五、基于Chrome插件：Prism Pretty

> A Chrome Extension to format/highlight/preview HTML/JS/CSS/Markdown code with Prism.js



### 1. 插件预览

该插件可用于高亮显示html、js、css以及markdown语法。

实际体验后，html、js、css展示效果很不错，特别markdown语法展示效果意外的好。感觉整体插件做工精致、功能强大。不过，目前markdown代码预览样式目前无法设置。

插件设置界面如下图所示：

![Prism Pretty截图](http://o7kubqw1j.bkt.clouddn.com/asserts/images/article/代码高亮类库Prism/screenshot.png)



### 2. 内容预览

试用了一下该插件，意外发现插件的一个强大功能：居然还能预览颜色代码内容、长度代码内容、图片链接内容等。

大家感受一下效果：

#### 预览颜色代码

 ![预览颜色代码](http://o7kubqw1j.bkt.clouddn.com/asserts/images/article/代码高亮类库Prism/Snip20160825_57.png)

#### 预览时间代码

 ![预览时间代码](http://o7kubqw1j.bkt.clouddn.com/asserts/images/article/代码高亮类库Prism/Snip20160825_60.png)

#### 预览长度代码
 ![预览长度代码](http://o7kubqw1j.bkt.clouddn.com/asserts/images/article/代码高亮类库Prism/Snip20160825_58.png)

#### 预览图片链接
 ![预览图片链接](http://o7kubqw1j.bkt.clouddn.com/asserts/images/article/代码高亮类库Prism/Snip20160825_62.png)




## 六、参考文档

- [prismjs官网](http://prismjs.com/index.html)
- [Prism：轻量级的 Javascript 代码高亮库](http://blog.wpjam.com/m/prism/)
- [使用 Prism.js 实现漂亮的代码语法高亮](http://c7sky.com/syntax-highlighting-with-prismjs.html)
- [Prism Test drive](http://prismjs.com/test.html)
- [Prism Pretty说明文档](https://raw.githubusercontent.com/L3au/Prism-Pretty/master/README.md)
- [Prism Pretty](https://github.com/L3au/Prism-Pretty)