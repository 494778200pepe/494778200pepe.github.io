---
layout: post
title:  "Proguard混淆 对于View 和 Activity混淆问题"
date:   2019-07-20 10:56:00 +0800
categories: Android
tags: 混淆
author: pepe
description: 『 对于View 和 Activity混淆问题 』
---

> 对于 `View` 和 `Activity` 混淆问题：饿了么 的团队提供了一个鲜为人知的 `gradle` 插件 用来无伤混淆 `Activity` 和 `View`，这个项目叫 [Mess](https://github.com/eleme/Mess)

具体内容各位可以稍后自行去阅读其文档和教程，链接最后都还会附于末尾。简单来说，Mess 弥补了 `Proguard` 不能检索 `XML` 文件的缺点，帮 Proguard 完成了 `Activity` 和 `View` 的改名及 mapping。













参考：

[android 防破解, 代码混淆，代码保护 - 晕菜一员 - 博客园](https://www.cnblogs.com/CharlesGrant/p/7544311.html)

[饿了么全面混淆插件 Mess - 安卓巴士Android开发者门户](http://www.10tiao.com/html/597/201808/2651943416/1.html)

[Mess](https://github.com/eleme/Mess)

[ButterMess](https://github.com/peacepassion/ButterMess)

[Android Gradle 打包流程 - shouniezhe的专栏 - CSDN博客](https://blog.csdn.net/shouniezhe/article/details/95162422)

[关于Android混淆的开源框架Mess的学习与分析 - 加油，罗老师 - CSDN博客](https://blog.csdn.net/qq_35770354/article/details/82799049)