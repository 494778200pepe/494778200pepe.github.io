---
layout: post
title:  "Activity、View、Window 之间的关系"
date:   2019-09-17 09:25:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 Activity、View、Window 之间的关系 』
---

每个 Activity 包含了一个 Window 对象，这个对象是由 PhoneWindow 做的实现。
而 PhoneWindow 将 DecorView 作为了一个应用窗口的根 View，
这个 DecorView 又把屏幕划分为了两个区域：一个是 TitleView，一个是 ContentView，
而我们平时在 Xml 文件中写的布局正好是展示在 ContentView 中的。

![window]({{ site.baseurl }}/assets/images/android/window.png)


参考：

[Android 面试（八）：说说 Activity、View、Window 之间的关系吧 - 简书](https://www.jianshu.com/p/d7853c4aba9c)

Activity、View、Window的理解一篇文章就够了 - 简书
https://www.jianshu.com/p/5297e307a688?from=message&isappinstalled=0














