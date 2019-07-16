---
layout: post
title:  "Activity 切换时的动画 overridependingtransition方法使用"
date:   2019-07-16 13:59:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 Activity 切换时的动画 overridependingtransition方法使用 』
---

> 实现两个 Activity 切换时的动画。在Activity中使用，有两个参数：进入动画和出去的动画。

### **使用说明**

* 1、必须在 StartActivity()  或 finish() 之后立即调用。

* 2、而且在 2.1 以上版本有效

* 3、手机设置-显示-动画，要开启状态有效

```
startActivity(new Intent(MainActivity.this,SecondActivity.class));
overridePendingTransition(R.anim.fade_in, R.anim.fade_out);

// 或者

finish();
overridePendingTransition(R.anim.fade_in, R.anim.fade_out);
```

> Activity系统默认的进入动画是从右侧进入到左侧停止，退出动画是从左到右移动直到完全退出界面。如果要修改Activity进入和退出动画有两种方式。

### 第一种方式:overridePendingTransition方法

### **第二种方式:设置Activity的theme属性**

在values文件夹的styles.xml中增加样式
```
<style name="Anim_fade parent="android:Theme.Light.NoTitleBar.Fullscreen">
    <item name="android:windowAnimationStyle">@style/fade</item>
</style>

<style name="fade" parent="@android:style/Animation.Activity">
    <item name="android:activityOpenEnterAnimation">@anim/fade_in</item>
    <item name="android:activityOpenExitAnimation">@anim/fade_out</item>
    <item name="android:activityCloseEnterAnimation">@anim/fade_in</item>
    <item name="android:activityCloseExitAnimation">@anim/fade_out</item>
    
    // android:activityOpenEnterAnimation 一个activity创建进入的效果。

    // android:activityOpenExitAnimation    一个activity还没有finish()下退出效果, 比如有俩个activity A与B 首先启动A 然后再启动B 那么A还没有finish()  这时A的退出效果。

    // android:activityCloseEnterAnimation 表示上一个activity返回进入效果 比如有俩个activity A与B  B在最上面，B退出(finish)后 A重新进入的效果。

    // android:activityCloseExitAnimation    表示的是activity finish()之后的效果 比如有俩个activity A与B B退出后会被finish() 那么B的退出效果在这定义。
    
</style>
```
在AndroidManifest.xml文件中设置Activity的样式,
```
<activity
    Android:name=".SecondActivity"
    android:theme="@style/Anim_fade" >
</activity>
```

为了简洁通用，推荐使用第二种方式进行设置动画。使用第二种方式，有的机器虽然进入的动画是可用的，但是退出的动画无效，你必须使用第一种方式的重写finish方法实现退出动画。


### **常用动画**

##### fade_in

```
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromAlpha="0.0"
    android:toAlpha="1.0"
    android:duration="2000"
    android:interpolator="@android:anim/decelerate_interpolator" />
```

##### fade_out

```
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromAlpha="1.0"
    android:toAlpha="0.0"
    android:duration="2000"
    android:interpolator="@android:anim/decelerate_interpolator" />
```

##### left_in

```
<set android:shareInterpolator="false"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <translate android:fromXDelta="100%"
        android:toXDelta="0"
        android:duration="300"/>
</set>

```
##### right_out

```
<set android:shareInterpolator="false"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <translate android:fromXDelta="0" 
        android:toXDelta="100%"
        android:duration="200" />
</set>
```

##### rotate_down

```
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <rotate  android:fromDegrees="0"
            android:toDegrees="-180"
            android:pivotX="50%"
            android:pivotY="50%"
            android:duration="200"/>
</set>

```
##### rotate_up

```
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <rotate  android:fromDegrees="0"
           android:toDegrees="180"
            android:pivotX="50%"
            android:pivotY="50%"
            android:duration="200"/>
</set>
```



参考：

[Activity切换动画（开启/退出）的两种实现方式 - iblade的博客 - CSDN博客](https://blog.csdn.net/iblade/article/details/83313582)

[Activity 切换时的动画 overridependingtransition方法使用 - xiaozhang0414的博客 - CSDN博客](https://blog.csdn.net/xiaozhang0414/article/details/79027706)

Android Activity动画属性简介 - 大新博客 - 博客园](https://www.cnblogs.com/daxin/p/3516737.html)