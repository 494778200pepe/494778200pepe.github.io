---
layout: post
title:  "SurfaceView"
date:   2018-01-30 16:07:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 SurfaceView 』
---

### Activity 和 SurfaceView 的生命周期

1、程序打开
 * Activity 调用顺序:onCreate()->onStart()->onResume()
 * SurfaceView 调用顺序: surfaceCreated()->surfaceChanged()
 * `onCreate()->onStart()->onResume()->surfaceCreated()->surfaceChanged()`

2、程序关闭（按 BACK 键）
 * Activity 调用顺序:onPause()->onStop()->onDestory()
 * SurfaceView 调用顺序: surfaceDestroyed()
 * `onPause()->surfaceDestroyed()->onStop()->onDestory()`

3、程序切到后台（按 HOME 键）
 * Activity 调用顺序:onPause()->onStop()
 * SurfaceView 调用顺序: surfaceDestroyed()
 * `onPause()->surfaceDestroyed()->onStop()`

4、程序切到前台
 * Activity 调用顺序: `onRestart()->onStart()->onResume()`
 * SurfaceView 调用顺序: `surfaceChanged()->surfaceCreated()`
 
5、`SurfaceView.setVisibility(View.VISIBLE)`
 * `surfaceCreated()->surfaceChanged()`
 
6、`SurfaceView.setVisibility(View.GONE)`
 * `surfaceDestroyed()`

7、屏幕锁定（挂断键或锁定屏幕）
 * Activity 调用顺序: `onPause()`
 * SurfaceView 什么方法都不调用

8、屏幕解锁 
 * Activity 调用顺序: `onResume()`
 * SurfaceView 什么方法都不调用
 
9、跳转到一个非完全覆盖的activity时也不会调`surfaceDestroyed`，返回时也不会调`surfaceCreated`

参考：


[ Activity 和 SurfaceView 的生命周期](http://www.liuxiao.org/2016/12/android-activity-%E5%92%8C-surfaceview-%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F/)

[MipcaActivityCapture代码阅读------surfaceview的生命周期 - CSDN博客](https://blog.csdn.net/yuxmdef1/article/details/18046237)