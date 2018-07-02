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
当`activity`的配置发生改变时会重新创建`activity`。配置了该属性之后，将不会导致重复创建。

[Android configChanges的属性值和含义(详细) - CSDN博客](https://blog.csdn.net/qq_33544860/article/details/54863895)

### **android:allowTaskReparenting**
> `android:allowTaskReparenting`是任务调整属性，它表明这个任务重新被送到前台的时候，该应用程序所定义的`Activity`是否可以从被启动的任务中转移到有相同亲和力的任务中。

* 这个属性的数据类型是布尔型，它的取值只有`true`和`false`两种。它不是必须指定的属性，如果我们没有显示指定这个属性，那么它将被指定为默认值`false`。
* `<application>`和`<activity>`节点上都有这个属性可以配置。
* 如果将该属性配置在`<application>`节点上，并且没有在`<activity>`节点上配置的情况下，`<application>`节点上的值将会应用到每一个`<activity>`节点上。
* 反之，如果`<activity>`节点上配置了这个属性，则以`<activity>`节点上的值为准。

### **android:permission**


### **android:theme**

### **android:taskAffinity**



### **android:process**




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


































