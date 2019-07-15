---
layout: post
title:  "uses-feature和uses-permission区别"
date:   2019-07-15 11:04:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 uses-feature和uses-permission区别 』
---

### **uses-feature：**

`uses-feature`的作用更像是一个过滤器，`google play` 商店会根据该标签来过滤设备，比如用户在`uses-feature`中声明了要使用相机，这时候在`google play`商店中该app就不再对没有照相机的设备显示。但是，如果用户同时也设置了`uses-feature`的属性`android:required`为`false`的话，`google play`商店仍然会对没有照相机的设备显示该app。

### **uses-permission：**

`uses-permission`则像是一个权限助手，帮助app去向用户请求app需要使用的权限。

参考：

[uses-feature和uses-permission区别 - 简书](https://www.jianshu.com/p/659690ff8c54)

[android程序中的AndroidManifest.xml中的uses-feature详解 - 佳颖的专栏 - CSDN博客](https://blog.csdn.net/cao478208248/article/details/22931773)