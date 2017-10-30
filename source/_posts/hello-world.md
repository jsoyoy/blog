---
title: defineProperty详解
---
## Object.defineProperty ##
    var a= {}
    Object.defineProperty(a,"b",{
      value:123
    })
    console.log(a.b);//123

传入三个参数

1. 目标对象
2. 定义的属性或方法的名字
3. 目标属性锁用用的特性（descriptor）


### descriptor ###

	//默认值
    var a= {}
	Object.defineProperty(a,"b",{
	  value:123,          //属性值
	  writable:false,     //属性值是否可写 false为只读
	  enumerable:false,	  //是否能在for...in循环中被遍历出来
	  configurable:false  //总开关 false就不能再设置其他值了
	})
	console.log(a.b);//123