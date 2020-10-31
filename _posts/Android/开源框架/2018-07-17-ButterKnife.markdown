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

Android主项目和Module中R类的区别 - 简书
https://www.jianshu.com/p/ba127b56811b

[AOP 最后一块拼图 - AST 抽象语法树 —— 最轻量级的AOP方法 - 简书](https://www.jianshu.com/p/0f1c7b3e907f)

[拆 Jake Wharton 系列之 ButterKnife - 简书](https://www.jianshu.com/p/b8b59fb80554?url_type=39&object_type=webpage&pos=1)

[Android主流三方库源码分析（七、深入理解ButterKnife源码）](https://jsonchao.github.io/2019/01/13/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E4%B8%83%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3ButterKnife%E6%BA%90%E7%A0%81%EF%BC%89/)

