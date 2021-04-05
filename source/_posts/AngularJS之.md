---
title: AngularJS之$watch
date: 2016-11-23 17:13:11
tags:
---

今天遇到一个需求:`$scope`中一些特定的变量改变的时候，需要触发相应的事件。搜索一下，了解到`AngularJS`一个scope函数——`$watch`

- 简介
`$watch` 是`scope`自带的函数，用于监听模型变化，当你的模型部分发生变化时它会通知你。意思就是它可以帮助我们在每个scope中监视其中的变量变化。

- 使用
```js 
$scope.$watch(watchExpression, listener, objectEquality);
```

- 参数说明：

  - `watchExpression`: 监听的对象，它可以是一个$scope中的变量,或函数如`function(){return $scope.name}`。
  - `listener`: 当`watchExpression`变化时会被调用的函数或者表达式,它接收3个参数：newValue(新值), oldValue(旧值), scope(作用域的引用)
  - `objectEquality`：是否深度监听，如果设置为true,它告诉Angular检查所监控的对象中每一个属性的变化. 如果你希望监控数组的个别元素或者对象的属性而不是一个普通的值, 那么你应该使用它
例子
``` js
$scope.values={};
$scope.values.value1 = 'value1';
$scope.values.value2 = 'value2';
$scope.$watch("object",function() {
  .....
},true);
$timeout (function() {
  $scope.value1 = "value2";
  $scope.value2 = "value1";
}, 1000);
```