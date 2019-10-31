---
layout: post
title:  "DrawerLayout"
date:   2019-10-31 09:44:00 +0800
categories: Android
tags: MD
author: pepe
description: 『 DrawerLayout 』
---

`DrawerLayout` 是一个 `ViewGroup`，且是具备 侧滑菜单 效果的父控件，因此，它的布局一般只包含两个子布局：

* 一个 内容布局（通常作为第一个子View）和 一个 侧滑菜单布局（通常作为第二个子View），

* 这两种布局的区分在于android:layout_gravity属性的设置，

* 若作为 侧滑菜单布局，可以通过设置android:layout_gravity="start"，让子布局作为左侧侧滑菜单；

* 反之，android:layout_gravity="end"则让子布局作为右侧侧滑菜单；

* 内容布局 不能设置该属性。

`DrawerLayout` 的布局还是有一些需要注意的事项：

* 主内容视图无需成为第一个子视图。甚至可以出现多个子布局作为主视图（效果类似FrameLayout，其子视图会重叠在一起）。

* 主内容视图设置为匹配父视图的宽度和高度（match_parent）， 因为在抽屉式导航栏处于隐藏状态时， 它代表整个 UI。

* 抽屉式导航栏视图（左/右侧滑菜单栏）必须使用 android:layout_gravity 属性指定其水平重力。要支持左侧侧滑，请使用 "start"（而非 "left"）；要支持右侧侧滑，请使用 "end"。

* 抽屉式导航栏视图以 dp 为单位指定其宽度， 且高度与父视图相匹配。抽屉式导航栏的宽度不应超过 320dp，从而用户始终可以看到部分主内容。






参考：

[Material Design系列教程（9） - DrawLayout - 简书](https://www.jianshu.com/p/2f05e71769ad)

[Android Material Design系列之Navigation Drawer](https://mp.weixin.qq.com/s?__biz=MjM5NDkxMTgyNw==&mid=2653057604&idx=1&sn=76f119252836e20c263d17d08b452b67&scene=21#wechat_redirect)




















