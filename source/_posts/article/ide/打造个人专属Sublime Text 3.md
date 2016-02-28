title: 打造个人专属Sublime Text 3
tags:
  - sublime
categories:
  - 开发工具
date: 2016-01-25 14:28:00
---


## 一、前言

### 1. 下载与破解
当前可用的破解码如下：
``` txt
—– BEGIN LICENSE —–
Andrew Weber
Single User License
EA7E-855605
813A03DD 5E4AD9E6 6C0EEB94 BC99798F
942194A6 02396E98 E62C9979 4BB979FE
91424C9D A45400BF F6747D88 2FB88078
90F5CC94 1CDC92DC 8457107A F151657B
1D22E383 A997F016 42397640 33F41CFC
E1D0AE85 A0BBD039 0E9C8D55 E1B89D5D
5CDB7036 E56DE1C0 EFCC0840 650CD3A6
B98FC99C 8FAC73EE D2B95564 DF450523
—— END LICENSE ——
```

参见：
 - [Sublime Text 3 破解版 + 注册机 + 汉化包 + 教程](http://www.xiumu.org/note/sublime-text-3.shtml)
 - [Sublime Text 3 for Mac 3075 破解版 – Mac上强大的代码编辑神器](http://www.waitsun.com/sublime-text-3-for-mac.html)
 - [Sublime Text 3破解补丁](https://www.shuax.com/archives/SublimeText3Crack.html)


### 2. Sublime Text 2与Sublime Text 3区别
简单的说：
 - ST3支持在项目目录里面寻找变量提供了
 - 对标签页更好地支持（更多的命令和快捷键）
 - 加快了程序运行的速度
 - 更新了API，使用Python3.3

参见：
 - [Sublime Text3与Sublime Text 2的区别在哪里？](https://www.zhihu.com/question/25078353)
 - [Sublime Text 3 文档之迁移指南](http://tutorial.jingwentian.com/Sublime-Text-3-Documentation/porting_guide.html)


## 二、配置

### 1. 我的配置
``` json
{
	"ignored_packages":
	[
		"Vintage"
	],

	// 字体
	"font_size": 12,

	// 高亮当前行
	"highlight_line": true,

	// 新标签页打开文件
	"preview_on_click": false,

	//滚动能否超过结尾
	"scroll_past_end": false,

	// TAB缩进宽度
	"tab_size": 4,

	// 自动转换TAB为空格
	"translate_tabs_to_spaces": false,

	// 皮肤
	"theme": "Soda Dark 3.sublime-theme",

	// 关闭自动更新
	"update_check": false,

	// 自动换行
	"word_wrap": "true"
}
```

### 2. 关闭自动更新
SublimeText的升级挺快的，每次升级完之后都要重新破解一次主文件，不用重新注册，如果嫌更新太麻烦可以在用户配置中加入如下配置禁用自动更新：
``` json
"update_check": false
```

### 3. soda主题
参见：https://github.com/buymeasoda/soda-theme/

通过package controller安装Theme - Soda。

### 4. 记住打开的文件
``` json
"remember_open_files": false
```

<!-- more -->

### 5. Sublime Text 2 使用 Eclipse 快捷键
[Sublime Text User Preferences -- Eclipse shortcuts for Sublime Text](https://gist.github.com/ufologist/5590481#file-package-control-sublime-settings)
[打造属于自己的前端开发神器 -- 给Sublime Text加上Eclipse的光环](http://www.douban.com/note/276794943/)

``` json
[
    /**
     * 常用快捷键(Sublime默认)
     * --------------
     * 
     * 光标一个单词一个单词的移动
     * { "keys": ["super+left"], "command": "move", "args": {"by": "words", "forward": false} },
     * 按住shift来选文字时, 一个个单词的选而不是一个个字母
     * { "keys": ["super+shift+left"], "command": "move", "args": {"by": "words", "forward": false, "extend": true} },
     *
     * 类似光标一个个单词的移动
     * { "keys": ["alt+left"], "command": "move", "args": {"by": "subwords", "forward": false} },
     * { "keys": ["alt+shift+right"], "command": "move", "args": {"by": "subword_ends", "forward": true, "extend": true} },
     *
     * 缩进
     * { "keys": ["super+]"], "command": "indent" },
     * { "keys": ["super+["], "command": "unindent" },
     *
     * 删除整个单词
     * { "keys": ["super+backspace"], "command": "delete_word", "args": { "forward": false } },
     * { "keys": ["super+delete"], "command": "delete_word", "args": { "forward": true } },
     *
     * 行排序(例如选中几个JSON字段, 让这些字段名按字母顺序排序)
     * { "keys": ["f9"], "command": "sort_lines", "args": {"case_sensitive": false} },
     *
     * 参考
     * ----------------------
     * Using Sublime Text as your IDE
     * http://www.chromium.org/developers/sublime-text
     *
     * Web Development With Sublime Text 2
     * http://www.paulund.co.uk/web-development-with-sublime-text-2
     */

    // editor配置
    { "keys": ["super+v"], "command": "paste_and_indent" },
    { "keys": ["super+shift+v"], "command": "paste" },

    /**
     * 适配eclipse快捷键
     *
     * 下面这位仁兄早就有了这个想法
     * Eclipse shortcuts for Sublime Text 2
     * http://icoloma.blogspot.com/2011/10/eclipse-shortcuts-for-sublime-text-2.html
     */
    { "keys": ["alt+/"], "command": "auto_complete" },
    { "keys": ["super+i"], "command": "reindent" },
    // 当前行和下面一行交互位置
    { "keys": ["alt+up"], "command": "swap_line_up" },
    { "keys": ["alt+down"], "command": "swap_line_down" },
    // 复制当前行到上一行
    { "keys": ["super+alt+up"], "command": "duplicate_line" },
    // 复制当前行到下一行
    { "keys": ["super+alt+down"], "command": "duplicate_line" },
    // 删除整行
    { "keys": ["super+d"], "command": "run_macro_file", "args": {"file": "Packages/Default/Delete Line.sublime-macro"} },
    // 光标移动到指定行
    { "keys": ["super+l"], "command": "show_overlay", "args": {"overlay": "goto", "text": ":"} },
    // 快速定位到选中的文字
    { "keys": ["super+k"], "command": "find_under_expand_skip" },
    // { "keys": ["super+shift+x"], "command": "swap_case" },
    { "keys": ["super+shift+x"], "command": "upper_case" },
    { "keys": ["super+shift+y"], "command": "lower_case" },
    // 在当前行的下一行插入空行(这时鼠标可以在当前行的任一位置, 不一定是最后)
    { "keys": ["shift+enter"], "command": "run_macro_file", "args": {"file": "Packages/Default/Add Line.sublime-macro"} },
    // 定位到对于的匹配符(譬如{})(从前面定位后面时,光标要在匹配符里面,后面到前面,则反之)
    { "keys": ["super+shift+p"], "command": "move_to", "args": {"to": "brackets"} },
    // 这个命令默认使用的是super+shift+p
    { "keys": ["super+p"], "command": "show_overlay", "args": {"overlay": "command_palette"} },
    // outline
    { "keys": ["super+o"], "command": "show_overlay", "args": {"overlay": "goto", "text": "@"} },
    // 当前文件中的关键字(方便快速查找内容)
    { "keys": ["super+alt+o"], "command": "show_overlay", "args": {"overlay": "goto", "text": "#"} },
    // open resource
    { "keys": ["super+shift+r"], "command": "show_overlay", "args": {"overlay": "goto", "show_files": true} },
    // 文件内查找/替换
    { "keys": ["super+f"], "command": "show_panel", "args": {"panel": "replace"} },
    // 全局查找/替换, 在查询结果中双击跳转到匹配位置
    {"keys": ["super+h"], "command": "show_panel", "args": {"panel": "find_in_files"} },

    // plugin配置
    { "keys": ["alt+a"], "command": "alignment" },
    {"keys": ["super+shift+f"], "command": "js_format"}
]
```

## 三、插件

### 1. Package Control
最简单的方式是通过 ctrl+\`快捷键打开控制台，粘贴以下代码到控制台执行。
参见：[Package Control INSTALLATION](https://packagecontrol.io/installation#st3)

#### Sublime Text 3
``` python
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

#### Sublime Text 2
``` python
import urllib2,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')
```

### 2. 实用插件
- SideBarEnhancements左侧菜单栏增强
- ConvertToUTF8
- AutoFileName自动补全文件(目录)名
- BracketHighlighter突出显示括号/引号
- ColorHighlighter 背景显示16进制颜色
- DocBlockr 生成注释模板
- Sublime CodeIntel – 智能代码提示
- FreeMarker:Freemarker模板语言辅助
- SublimeCodeIntel 代码提示
- JSHint、JsFormart、jQuery
- Color Highlighter
- Indent XML
- sublimeLinter
- pretty json


参见：
 - [sublime Text3配置及快捷键、插件推荐总结](http://blog.csdn.net/xby1993/article/details/23995667)
 - [SublimeText2 快捷键一览表](http://blog.useasp.net/archive/2013/06/14/sublime-text-2-all-default-Shortcuts-table-on-windows-translated-with-chinese.aspx)

