---
layout: post
title:  "Android动画--Tween Animation 三"
date:   2018-11-09 11:35:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 rotate 』
---

### **xml实现**
```
<?xml version="1.0" encoding="utf-8"?>
<rotate xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromDegrees="0"
    android:toDegrees="-650"
    android:pivotX="50%"
    android:pivotY="50%"
    android:duration="3000"
    android:fillAfter="true"/>
```

属性说明：
```
    <!--rotate-->
    <!--android:fromDegrees     -->
    <!--开始旋转的角度位置，正值代表顺时针方向度数，负值代码逆时针方向度数-->
    <!--android:toDegrees         -->
    <!--结束时旋转到的角度位置，正值代表顺时针方向度数，负值代码逆时针方向度数-->
    <!--android:pivotX               -->
    <!--缩放起点X轴坐标，可以是数值、百分数、百分数p 三种样式，比如 50、50%、50%p，具体意义已在scale标签中讲述，这里就不再重讲-->
    <!--android:pivotY               -->
    <!--缩放起点Y轴坐标，可以是数值、百分数、百分数p 三种样式，比如 50、50%、-->
```

### **开始动画**
```
Animation aa3 = AnimationUtils.loadAnimation(this, R.anim.rotateanim);
image.startAnimation(aa3);
```

### **Java构造**
```
// 缩放起点X轴坐标，可以是数值、百分数、百分数p 三种样式，比如 50、50%、50%p.
// 当为数值时，mPivotXType = ABSOLUTE = 0,表示在当前View的左上角，即原点处加上50px，做为起始缩放点,整数；
// 如果是50%，mPivotXType = RELATIVE_TO_SELF = 1,表示在当前控件的左上角加上自己宽度的50%做为起始点,浮点数；
// 如果是50%p，mPivotXType = RELATIVE_TO_PARENT = 2,那么就是表示在当前的左上角加上父控件宽度的50%做为起始点x轴坐标,浮点数。
// 默认mPivotXType = ABSOLUTE;
RotateAnimation aa2 = new RotateAnimation(0, 360,2,0.5f,2,0.5f);
aa2.setDuration(3000);
image.startAnimation(aa2);
```                




























