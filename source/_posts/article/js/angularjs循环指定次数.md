title: angularjs循环指定次数
tags:
  - angularjs
categories:
  - 前端设计
date: 2016-03-13 10:16:00
---

<img src="/asserts/images/angularjs.png" class="img-logo img-center" />


## 一、js代码
``` javascript
$scope.range = function(n) {
    return new Array(n);
};
```
通过new Array(n)创建指定大小容量的数组。


## 二、html代码
``` html
<div class="list-group-horizontal">
  <a ng-repeat="i in range(6) track by $index">
	<span ng-bind="$index + 1"></span>
  </a>
</div>
```

遍历数组，并通过track by $index来指定数组索引为$index。


## 三、输出结果
输出结果为：1 2 3 4 5 6


## 四、参考文档
[Way to ng-repeat defined number of times instead of repeating over array?](http://stackoverflow.com/questions/16824853/way-to-ng-repeat-defined-number-of-times-instead-of-repeating-over-array)
