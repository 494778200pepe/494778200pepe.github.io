---
layout: post
title:  "SparseArrayCompat"
date:   2018-05-12 11:28:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 SparseArrayCompat 』
---
### SparseArrayCompat

`SparseArrayCompat`其实是一个map容器,它使用了一套算法优化了hashMap;

* 优点：可以节省至少50%的缓存.
* 缺点：有局限性只针对下面类型：`key: Integer; value: object`



[Android开发中高效的数据结构用SparseArray代替HashMap - fancychendong的专栏 - 博客频道 - CSDN.NET](http://blog.csdn.net/fancylovejava/article/details/45148325)

Android内存优化（使用SparseArray和ArrayMap代替HashMap） - CSDN博客
https://blog.csdn.net/u010687392/article/details/47809295

Android编程之SparseArray<E>详解 - CSDN博客
https://blog.csdn.net/pi9nc/article/details/11352491

Android中的HashMap,ArrayMap和SparseArray - 简书
https://www.jianshu.com/p/aff3b8990ab3

SparseArray - Android SDK | Android Developers
http://www.android-doc.com/reference/android/util/SparseArray.html

GC: SparseArray - android.util.SparseArray (.java) - GrepCode Class Source
http://www.grepcode.com/file/repository.grepcode.com/java/ext/com.google.android/android/5.1.1_r1/android/util/SparseArray.java?av=f