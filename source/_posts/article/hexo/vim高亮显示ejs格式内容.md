title: vim高亮显示ejs格式内容
tags:
  - hexo
  - vim
categories:
  - 博客搭建
date: 2016-02-16 11:51:00
---

## 一、简介
[官方介绍](http://www.embeddedjs.com/)如下：
> EJS is a client-side templating language that was originally part of JavaScriptMVC, which has now been replaced by DoneJS.


## 二、解决方法

### 方法一 视ejs格式为html格式
``` bash
au BufNewFile,BufRead *.ejs set filetype=html
```

详细au命令参见：[AUTOCMD](http://vimcdoc.sourceforge.net/doc/autocmd.html)


### 方法二 定制ejs格式语法高亮

#### 1. 下载ejs语法文件
下载ejs.vim文件到~/.vim/syntax/目录。
下载地址：https://github.com/emilis/emilis-config/blob/master/.vim/syntax/ejs.vim

文件内容如下：
``` bash
runtime! syntax/html.vim
unlet b:current_syntax

" Include Java syntax
syn include @ejsJavaScript syntax/javascript.vim

syn region ejsScriptlet matchgroup=ejsTag start=/<%/  keepend end=/%>/ contains=@ejsJavaScript
syn region ejsExpr	matchgroup=ejsTag start=/<%=/ keepend end=/%>/ contains=@ejsJavaScript

" Redefine htmlTag so that it can contain jspExpr
syn clear htmlTag
syn region htmlTag start=+<[^/%]+ end=+>+ contains=htmlTagN,htmlString,htmlArg,htmlValue,htmlTagError,htmlEvent,htmlCssDefinition,@htmlPreproc,@htmlArgCluster,ejsExpr,javaScript


" syn keyword ejsPrint contained print
syn match javaScriptType        /\<\zsvars\ze\./
syn match javaScriptSpecial     /\<\zsexports\ze\./
syn match javaScriptFunction    /\<\zsprint\ze(/
syn match javaScriptFunction    /\<\zsinclude\ze(/
syn match javaScriptFunction    /\<\zsincludeObject\ze(/
syn match javaScriptFunction    /\<\zsfetch\ze(/
syn match javaScriptFunction    /\<\zsfetchObject\ze(/

command -nargs=+ HiLink hi def link <args>
HiLink  ejsTag      htmlTag
delcommand HiLink

let b:current_syntax = "ejs"
```

#### 2. 设置ejs格式语法
``` bash
 au BufNewFile,BufRead *.ejs set filetype=ejs
```


## 三、参考文档
[Syntax highlight for .ejs files in vim](http://stackoverflow.com/questions/4597721/syntax-highlight-for-ejs-files-in-vim)
