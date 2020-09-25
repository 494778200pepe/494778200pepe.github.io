---
layout: post
title:  "Jetpack - ViewModel"
date:   2020-09-24 16:24:00 +0800
categories: Android
tags: Jetpack
author: pepe
description: 『 ViewModel 』
---

官方文档：

[ViewModel 概览  Android Developers](https://developer.android.google.cn/topic/libraries/architecture/viewmodel)

痛点：

* 如果系统销毁或重新创建界面控制器，则存储在其中的任何临时性界面相关数据都会丢失。对于简单的数据，Activity 可以使用 `onSaveInstanceState()`方法从`onCreate()`中的捆绑包恢复其数据，但此方法仅适合可以序列化再反序列化的少量数据，而不适合数量可能较大的数据，如用户列表或位图。

* 界面控制器经常需要进行异步调用，这些调用可能需要一些时间才能返回结果。界面控制器需要管理这些调用，并确保系统在其销毁后清理这些调用以避免潜在的内存泄露。此项管理需要大量的维护工作，并且在因配置更改而重新创建对象的情况下，会造成资源的浪费，因为对象可能需要重新发出已经发出过的调用。
	
* `Activity`和`Fragment`承载过多，代码膨胀。需要将从数据库或网络加载数据的任务分离出去。

### **ViewModel：**

> 架构组件为界面控制器提供了`ViewModel`辅助程序类，该类负责为界面准备数据。在配置更改期间会自动保留`ViewModel`对象，以便它们存储的数据立即可供下一个`Activity`或`Fragment`实例使用。例如，如果您需要在应用中显示用户列表，请确保将获取和保留该用户列表的责任分配给 `ViewModel`，而不是`Activity`或`Fragment`.





参考：

[从源码看 Jetpack（6）-ViewModel源码详解](https://juejin.im/post/6873356946896846856)

[Jetpack使用（四）ViewModel核心原理 - 简书](https://www.jianshu.com/p/00799899fe7b)


