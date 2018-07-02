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
### **android:theme**
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

### **android:enabled**
`activity`是否可以被Android系统自动实例化

> 默认情况下，Android系统会自行实例化每一个应用程序的组件，包括Android四大组件，但如果我们需要自己完成这些事情的话，就需要使用`android:enabled`属性来限制Android系统的行为。这个属性表明Android系统是否可以被实例化应用程序组件，如果其值为`true`，则说明应用程序组件可以被Android系统自动实例化；如果为`false`，则说明实例化组件的工作需要手工完成。该属性的默认值为`true`。每一个组件都可以单独定义自己的`enabled`属性。如果这个属性定义在`<application>`节点中，那么它会默认将每个组件的`enabled`属性设置为相同的值。如果每一个组件单独定义了这个属性，那么`<application>`节点上定义的属性对此组件不再生效，就由自己的`enabled`属性决定。

### **android:permission**


### **android:taskAffinity**



### **android:process**




### **android:alwaysRetainTaskState**
> 系统是否永久保存当前`Activity`所在的`Task`的状态。`true`将会保存，`false`系统将允许在明确的情况下把`Task`的状态重置为初始状态。默认值是`false`。这个属性只对这个`Task`的`root activity`有意义，它将忽略其它所有的`activity`。
通常情况下，当用户从主屏幕重新选择这个`Task`时系统将清除这个`Task`(移除`root activity`之上的所有`activity`)。通常，如果用户在确定的时间内没有再访问这个`Task`，例如30分钟，系统将执行清除操作。
可是，当这个属性设置为`true`时，用户将总是回到这个`Task`的最后状态，无论他们采取任何方式。这是非常有用的，例如，像网页浏览器这样的应用程序有很多的状态（例如有很多tab），用户不想忘记他们在浏览哪个网页。

### **android:clearTaskOnLanunch**




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








[Android manifest属性总结 - 飞天小鳄 - 博客园](https://www.cnblogs.com/ftxe/p/3642458.html)

[AndroidManifest中android:persistent属性研究 - CSDN博客](https://blog.csdn.net/a87636764/article/details/50330507)

[AndroidManifest属性 - CSDN博客](https://blog.csdn.net/lty406910111/article/details/70226212)

[AndroidManifest.xml中一些常用的属性 - CSDN博客](https://blog.csdn.net/qinxiaoli0309a/article/details/52755314)
































