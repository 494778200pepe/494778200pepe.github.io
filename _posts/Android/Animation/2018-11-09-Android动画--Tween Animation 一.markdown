---
layout: post
title:  "Android动画--Tween Animation 一"
date:   2018-11-09 10:08:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 alpha 』
---

> Tween Animation(补间动画)有四种，分别是：alpha(淡入淡出)、translate(位移)、scale(缩放大小)、rotate(旋转)。

补间动画有两个实现方式，一种是加载xml中的动画，一种是纯Java构造。

先来说说`alpha`.

### **xml实现**
```
<?xml version="1.0" encoding="utf-8"?>
<!--透明度控制动画效果 -->
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromAlpha="0"
    android:toAlpha="1.0"
    android:duration="3000"/>
```

属性说明：
```
    <!--fromAlpha 属性为动画起始时透明度  0.0表示完全透明-->
    <!--toAlpha   属性为动画结束时透明度  1.0表示完全不透明-->
    <!--以上值取0.0-1.0之间的float数据类型的数字-->
```






























