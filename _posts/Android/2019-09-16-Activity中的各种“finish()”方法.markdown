---
layout: post
title:  "Activity中的各种“finish()”方法"
date:   2019-09-15 10:40:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 Activity中的各种“finish()”方法 』
---

### **finish ()**

finish ()方法在你的activity结束或者应该被关闭时调用。ActivityResult将通过onActivityResult()方法传递给启动者。这是比较常用的关闭Activity的方法。

> 注意：通过startActivityForResult方法来启动Activity，才能将ActivityResult通过onActivityResult()方法传递给启动者。普通的startActivity方法是不会在 finish ()方法后传递ActivityResult的。

### **finishActivity (int requestCode)**

强制关闭另一个你先前通过startActivityForResult(Intent, int)启动的Activity。该方法不会关闭当前Activity，可以通过requestCode关闭，先前通过startActivityForResult传递过相同requestCode打开的Activity。

> 注意：通过这个方法，我们还可以关闭一起我们可以打开但不能通过代码操作的页面，比如其他应用或者系统界面。


### **finishAffinity**

关闭该Activity和同一栈中的所有位于该Activity下面的Activity。比如说在同一Activity栈中，Activity A启动了Activity B，Activity B启动了Activity C。Activity B调用finishAffinity()方法，会关闭 Activity A和 Activity B，Activity C仍然存在。如果Activity C调用该方法，则A，B，C，都会被关闭，且如果应用只有这一个栈，那么C调用该方法会直接退出应用。

> 注意：这个方法不允许你返回结果给前一个activity，如果你这样做的话会抛出异常。该方法在API level 16之后添加。

退出应用的第N+1种方法-一行代码退出应用 - gongziwushuang的博客 - CSDN博客
https://blog.csdn.net/gongziwushuang/article/details/51970015?utm_source=blogxgwz8

### **finishAfterTransition**

翻转Activity进入转场动画（Transition）用于Activity退出。

> 注意：该方法 在API level 21之后添加，使用它时，你得先定义自己的转场动画，否则它的作用和finish()方法没有区别。这里的转场动画不是指由overridePendingTransition实现的动画，而是通过ActivityOptions类实现的转场动画。

### **finishAndRemoveTask**

关闭Activity且关闭该Activity作为根Activity的的任务。

> 注意：该方法在API level 21之后添加。


### **finishActivityFromChild (Activity child, int requestCode)**

当一个该Activity的子activity调用它的finishActivity()方法时调用。

> 注意：该方法我只在使用TabActivity时，调用其子Activity后调用了finishActivity()方法，其他调用情况没有查出来。（TabActivity在API level 13时废弃了）

### **finishFromChild**

当一个该Activity的子activity调用它的finish()方法时调用。

> 注意：该方法和finishActivityFromChild 方法一样，我只在使用TabActivity时有看到调用。

参考：

[Activity中的各种“finish()”方法 - 简书](https://www.jianshu.com/p/89091f9cd29b)
















