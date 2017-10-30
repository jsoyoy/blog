---
title: promise的理解
---

## promise  ##

1.首先，是个对象 new Promise

2.接受两个参数，resolve/reject

3.Promise实例生成后，then方法可指定不同状态的回调函数

	//生成实例 上下文属于同步执行
	var promise = new Promise(function(resolve, reject) {
	  // ... some code
	
	  if (/* 异步操作成功 */){
	    resolve(value);
	  } else {
	    reject(error);
	  }
	});

	//then的调用 异步执行
	promise.then(function(value) {
	  // success
	}, function(error) {
	  // failure
	});
