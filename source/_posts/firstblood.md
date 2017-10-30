---
title: Javascript设计模式 - 面向对象编程
---

# 创建一个类 #

## this添加的属性和proptotype添加的属性区别 ##
this定义的属性或是对象自身拥有的，每次通过类创建新对象时，this定义的属性和方法也会得到相应的创建；
prototype这是继续的属性和方法，通过prototype能访问到，这样每次创建的新对象就不会再次创建； 