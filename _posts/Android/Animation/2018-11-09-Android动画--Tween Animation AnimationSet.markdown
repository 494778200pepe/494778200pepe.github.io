---
layout: post
title:  "Android动画--Tween Animation AnimationSet"
date:   2018-11-09 13:46:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 AnimationSet 』
---

### **xml实现**
```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="3000"
    android:fillAfter="true">
    <alpha
        android:fromAlpha="0.0"
        android:toAlpha="1.0"/>
    <scale
        android:fromXScale="0.0"
        android:toXScale="1.4"
        android:fromYScale="0.0"
        android:toYScale="1.4"
        android:pivotX="50%"
        android:pivotY="50%"/>
    <rotate
        android:fromDegrees="0"
        android:toDegrees="720"
        android:pivotX="50%"
        android:pivotY="50%"/>
</set>


```

属性说明：
```
    <!--Android:interpolator="@android:anim/accelerate_interpolator"-->
    <!--android:shareInterpolator="true"-->
    <!--这样所有的Animation共用一个Interpolator-->
```

### **开始动画**
```
Animation aa0 = AnimationUtils.loadAnimation(this, R.anim.setanim);
image.startAnimation(aa0);
```

### **Java构造**
```
AnimationSet as=new AnimationSet(false);//设置shareInterpolator=false
AlphaAnimation aa=new AlphaAnimation(0,1);
ScaleAnimation sa = new ScaleAnimation(0, 1,0,1);
as.addAnimation(aa);
as.addAnimation(sa);
as.setDuration(3000);//默认duration为0
image.startAnimation(as);
```                




























