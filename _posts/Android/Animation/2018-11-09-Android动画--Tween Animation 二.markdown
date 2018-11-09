---
layout: post
title:  "Android动画--Tween Animation 二"
date:   2018-11-09 11:30:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 scale 』
---

### **xml实现**
```
<?xml version="1.0" encoding="utf-8"?>
<scale xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromXScale="0.0"
    android:toXScale="1.0"
    android:fromYScale="0.0"
    android:toYScale="1.0"
    android:pivotX="50"
    android:pivotY="50"
    android:duration="3000" />
```

属性说明：
```
    <!--scale特有属性-->
    <!--浮点型值：-->
    <!--fromXScale 属性为动画起始时 X坐标上的伸缩尺寸-->
    <!--toXScale   属性为动画结束时 X坐标上的伸缩尺寸-->
    <!--fromYScale 属性为动画起始时Y坐标上的伸缩尺寸-->
    <!--toYScale   属性为动画结束时Y坐标上的伸缩尺寸-->
    <!--说明:以上四种属性值
    0.0表示收缩到没有
    1.0表示正常无伸缩
    值小于1.0表示收缩
    值大于1.0表示放大-->

    <!--pivotX     属性为动画相对于物件的X坐标的开始位置-->
    <!--pivotY     属性为动画相对于物件的Y坐标的开始位置-->
    <!--说明:   以上两个属性值 可取值50、50%、50%p-->
    <!--50表示绝对值50px-->
    <!--50%表示自身的50%-->
    <!--50%p表示父控件的50%-->
```

### **开始动画**
```
Animation aa3 = AnimationUtils.loadAnimation(this, R.anim.scaleanim);
image.startAnimation(aa3);
```

### **Java构造**
```
// 缩放起点X轴坐标，可以是数值、百分数、百分数p 三种样式，比如 50、50%、50%p.
// 当为数值时，mPivotXType = ABSOLUTE = 0,表示在当前View的左上角，即原点处加上50px，做为起始缩放点,整数；
// 如果是50%，mPivotXType = RELATIVE_TO_SELF = 1,表示在当前控件的左上角加上自己宽度的50%做为起始点,浮点数；
// 如果是50%p，mPivotXType = RELATIVE_TO_PARENT = 2,那么就是表示在当前的左上角加上父控件宽度的50%做为起始点x轴坐标,浮点数。
// 默认mPivotXType = ABSOLUTE;
ScaleAnimation aa2 = new ScaleAnimation(0, 1,0,1,1,2,1,2);
aa2.setDuration(3000);
image.startAnimation(aa2);
```                




























