

FATAL Cannot set property 'lastIndex' of undefined
TypeError: Cannot set property 'lastIndex' of undefined
    at highlight (/Users/yeshaoting/java/workspace/github/hexo-blog/node_modules/hexo/node_modules/hexo-util/node_modules/highlight.js/lib/highlight.js:512:35)
    at /Users/yeshaoting/java/workspace/github/hexo-blog/node_modules/hexo/node_modules/hexo-util/node_modules/highlight.js/lib/highlight.js:562:21
    at Array.forEach (native)

修改 _config.yml里 highlight下的auto_detect为false.

参见：https://github.com/hexojs/hexo/issues/1627