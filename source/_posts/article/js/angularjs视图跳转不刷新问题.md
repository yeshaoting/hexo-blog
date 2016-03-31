title: angularjs视图跳转不刷新问题
tags:
  - angularjs
categories:
  - 前端设计
date: 2016-03-09 15:56:00
---

<img src="/asserts/images/logo/angularjs.png" class="img-logo img-center" />


在实际工作过程中，发现通过 `$state.go("stage2")` 让视图跳转到stage2，但视图内容并不刷新。

## 一、问题解决

### 1. 方法一
通过修改地址栏内容，模板一次用户通过地址栏输入地址操作。

``` javascript
//be sure to inject $scope and $location
var changeLocation = function(url, forceReload) {
  $scope = $scope || angular.element(document).scope();
  if(forceReload || $scope.$$phase) {
    window.location = url;
  }
  else {
    //only use this if you want to replace the history stack
    //$location.path(url).replace();

    //this this if you want to change the URL and add it to the history stack
    $location.path(url);
    $scope.$apply();
  }
};
```

### 2. 方法二
通过自带的参数reload，要求视图跳转时强制刷新视图内容。
``` javascript
$state.go("stage2", {}, {"reload": true});
```

或者视图跳转完后，通过reloa()方法刷新视图。
``` javascript
$state.go("stage2");
$state.reload("stage2");
```


## 二、参考文档
- [In Angular, how to redirect with $location.path as $http.post success callback](http://stackoverflow.com/a/14387747)
- [$state](http://angular-ui.github.io/ui-router/site/#/api/ui.router.state.$state)