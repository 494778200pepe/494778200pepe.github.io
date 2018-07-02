---
layout: post
title:  "Android Manifest属性"
date:   2018-07-02 10:51:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 Android Manifest属性 』
---
### **android:label**
### **android:icon**
### **android:name**
### **android:launchMode**
android 启动模式：

* "standard" 
* "singleTop" 
* "singleTask"
* "singleInstance" 

### **android:screenOrientation**

* `landscape`: 横屏显示
* `portrait`: 竖屏显示

### **android:windowSoftInputMode**
设置窗体软键盘交互模式。

[android:windowSoftInputMode属性具体解释 - gccbuaa - 博客园](https://www.cnblogs.com/gccbuaa/p/7049889.html)

### **android:configChanges**
当activity的配置发生改变时会重新创建activity。配置了该属性之后，将不会导致重复创建。

[Android configChanges的属性值和含义(详细) - CSDN博客](https://blog.csdn.net/qq_33544860/article/details/54863895)

### **android:permission**


### **android:theme**

### **android:taskAffinity**



### **android:process**


### **android:allowTaskReparenting**
### **android:alwaysRetainTaskState**
### **android:clearTaskOnLanunch**


### **android:enabled**
activity是否可以被实例化
### **android:excludeFromRecents**
是否可被显示在最近打开的activity列表里
### **android:exported**
是否允许activity被其他程序调用
### **android:finishOnTaskLaunch**
是否关闭已打开的activity，当用户重新启动这个任务的时候
### **multiprocess**
允许多进程
### **android:onHistory**


### **android:stateNotNeeded**








Android manifest属性总结 - 飞天小鳄 - 博客园
https://www.cnblogs.com/ftxe/p/3642458.html


AndroidManifest中android:persistent属性研究 - CSDN博客
https://blog.csdn.net/a87636764/article/details/50330507


































