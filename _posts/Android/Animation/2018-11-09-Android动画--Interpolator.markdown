---
layout: post
title:  "Android动画--Interpolator"
date:   2018-11-09 13:56:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 Interpolator 』
---

> Interpolator:差值器，通俗来说就是速度曲线。

### **AccelerateDecelerateInterpolator**

在动画开始与结束的地方速率改变比较慢，在中间的时候加速

### **AccelerateInterpolator**

在动画开始的地方速率改变比较慢，然后开始加速

### **AnticipateInterpolator**

开始的时候向后然后向前甩

### **AnticipateOvershootInterpolator**

开始的时候向后然后向前甩一定值后返回最后的值

### **BounceInterpolator**

动画结束的时候弹起

### **CycleInterpolator**

动画循环播放特定的次数，速率改变沿着正弦曲线

### **DecelerateInterpolator**

在动画开始的地方快然后慢

### **LinearInterpolator**

以常量速率改变

### **OvershootInterpolator**

向前甩一定值后再回到原来位置


[Android Animations Tutorial 5: More on Interpolators ](http://cogitolearning.co.uk/2013/10/android-animations-tutorial-5-more-on-interpolators/)






















