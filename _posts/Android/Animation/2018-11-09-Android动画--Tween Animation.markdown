---
layout: post
title:  "Android动画--Tween Animation"
date:   2018-11-09 12:15:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 Tween Animation 通用属性 』
---

### **Tween Animation 共用属性**
```                
    <!--Animation共有属性-->
    <!--长整型值：-->
    <!--android:duration-->
    <!--动画持续时间，以毫秒为单位-->
    
    <!--布尔型值:-->
    <!--android:fillAfter-->
    <!--如果设置为true，控件动画结束时，将保持动画最后时的状态-->
    
    <!--布尔型值:-->
    <!--android:fillBefore-->
    <!--如果设置为true,控件动画结束时，还原到开始动画前的状态-->
    <!--android:fillEnabled-->
    <!--当设置为true时，fillAfter和fill, Befroe将会都为true，此时会忽略fillBefore 和fillAfter两种属性-->
    
    <!--android:repeatCount-->
    <!--重复次数-->
    
    <!--android:repeatMode-->
    <!--重复类型，有reverse和restart两个值，reverse表示倒序回放，restart表示重新放一遍，必须与repeatCount一起使用才能看到效果。-->
    <!--因为这里的意义是重复的类型，即回放时的动作。-->
    
    <!--android:interpolator  -->
    <!--设定插值器，其实就是指定的动作效果，比如弹跳效果等。-->
    
    <!--boolean-->
    <!--android:detachWallpaper-->
    <!--是否在壁纸上运行-->
    
    <!--long-->
    <!--android:startOffset-->
    <!--动画之间的时间间隔，从上次动画停多少时间开始执行下个动画-->
    <!--调用start函数之后等待开始运行的时间，单位为毫秒-->
    
    <!--int-->
    <!--android:zAdjustment-->
    <!--表示被设置动画的内容运行时在Z轴上的位置（top/bottom/normal），默认为normal-->
    <!--[normal] or [top] or [bottom]-->
    <!--定义动画的Z Order的改变
        0：保持Z Order不变
        1：保持在最上层
        -1：保持在最下层 -->
```



























