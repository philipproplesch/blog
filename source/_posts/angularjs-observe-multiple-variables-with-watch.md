---
title: "AngularJS: Observe multiple variables with $watch()"
date: 2014-04-26
tags: angularjs
---
If you want to observe a variable this can be done easily by using the [`$watch` function](http://docs.angularjs.org/api/ng/type/$rootScope.Scope#$watch).

    $scope.$watch('foo', function(value) {
      // will be triggered when $scope.foo will be changed
    });
    
In order to observe multiple properties all you need to do is concatenate them with a `+`.
    
    $scope.$watch('foo + bar', function(value) {
      //  do something if $scope.foo or $scope.bar changes
    });
