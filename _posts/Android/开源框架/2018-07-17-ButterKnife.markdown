---
layout: post
title:  "ButterKnife"
date:   2018-07-17 16:14:00 +0800
categories: Android
tags: 开源框架
author: pepe
description: 『 ButterKnife 』
---

* 1、apt 编译时，生成代码

* 2、运行时，`ButterKnife.bind(this);`通过 `Activity` 的 `name + _ViewBinding` 得到 `MainActivity1_ViewBinding` 的 class 对象

* 3、利用反射，得到 class 对象的构造方法，创建对象并返回 `Unbinder(接口)` 

* 4、在 `MainActivity1_ViewBinding` 类中，apt 已经通过注解生成了相应的 `findViewById` 方法，对 activity 的成员变量进行赋值

* 5、使用 ButterKnife 注解的成员变量，不能是 `private`，否则在 `MainActivity1_ViewBinding` 类中，就无法直接对 activity 的成员变量进行赋值

参考：

[AOP 最后一块拼图 - AST 抽象语法树 —— 最轻量级的AOP方法 - 简书](https://www.jianshu.com/p/0f1c7b3e907f)

[拆 Jake Wharton 系列之 ButterKnife - 简书](https://www.jianshu.com/p/b8b59fb80554?url_type=39&object_type=webpage&pos=1)



