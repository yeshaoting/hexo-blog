title: High一下
tags:
  - hexo
categories:
  - 博客搭建
date: 2016-02-16 18:06:00
---

<img src="/asserts/images/logo/hexo.png" class="img-logo img-center" />

## 一、前言
本文内容基于wuchong的Jacman主题。

让网站元素动起来，从[酷壳](http://coolshell.cn/)网站抄过来的，套到hexo博客菜单里。


## 二、代码
代码：themes/jacman/layout/_partial/header.ejs

找到 ==theme.menu== 位置，代码在菜单遍历输出后，添加如下代码：
``` html
<li><a title="把这个链接拖到你的Chrome收藏夹工具栏中" href='javascript:(function(){function c(){var e=document.createElement("link");e.setAttribute("type","text/css");e.setAt    tribute("rel","stylesheet");e.setAttribute("href",f);e.setAttribute("class",l);document.body.appendChild(e)}function h(){var e=document.getElementsByClassName(l);for(var t=0;t<e.length;t++){document    .body.removeChild(e[t])}}function p(){var e=document.createElement("div");e.setAttribute("class",a);document.body.appendChild(e);setTimeout(function(){document.body.removeChild(e)},100)}function d(e    ){return{height:e.offsetHeight,width:e.offsetWidth}}function v(i){var s=d(i);return s.height>e&amp;&amp;s.height<n&amp;&amp;s.width>t&amp;&amp;s.width<r}function m(e){var t=e;var n=0;while(!!t){n+=t    .offsetTop;t=t.offsetParent}return n}function g(){var e=document.documentElement;if(!!window.innerWidth){return window.innerHeight}else if(e&amp;&amp;!isNaN(e.clientHeight)){return e.clientHeight}re    turn 0}function y(){if(window.pageYOffset){return window.pageYOffset}return Math.max(document.documentElement.scrollTop,document.body.scrollTop)}function E(e){var t=m(e);return t>=w&amp;&amp;t<=b+w}    function S(){var e=document.createElement("audio");e.setAttribute("class",l);e.src=i;e.loop=false;e.addEventListener("canplay",function(){setTimeout(function(){x(k)},500);setTimeout(function(){N();p    ();for(var e=0;e<O.length;e++){T(O[e])}},15500)},true);e.addEventListener("ended",function(){N();h()},true);e.innerHTML=" <p>If you are reading this, it is because your browser does not support the     audio element. We recommend that you get a new browser.</p> <p>";document.body.appendChild(e);e.play()}function x(e){e.className+=" "+s+" "+o}function T(e){e.className+=" "+s+" "+u[Math.floor(Math.r    andom()*u.length)]}function N(){var e=document.getElementsByClassName(s);var t=new RegExp("\\b"+s+"\\b");for(var n=0;n<e.length;){e[n].className=e[n].className.replace(t,"")}}var e=30;var t=30;var n    =350;var r=350;var i="//s3.amazonaws.com/moovweb-marketing/playground/harlem-shake.mp3";var s="mw-harlem_shake_me";var o="im_first";var u=["im_drunk","im_baked","im_trippin","im_blown"];var a="mw-st    robe_light";var f="//s3.amazonaws.com/moovweb-marketing/playground/harlem-shake-style.css";var l="mw_added_css";var b=g();var w=y();var C=document.getElementsByTagName("*");var k=null;for(var L=0;L<    C.length;L++){var A=C[L];if(v(A)){if(E(A)){k=A;break}}}if(A===null){console.warn("Could not find a node of the right size. Please try a different page.");return}c();S();var O=[];for(var L=0;L<C.leng    th;L++){var A=C[L];if(v(A)){O.push(A)}}})()'>High一下!</a></li>
```

