---
layout: post
title:  "Android动画--Animator"
date:   2018-11-10 13:49:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 Animator 』
---

### **简单使用**
```
// scale_anim.xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:ordering="together" >

    <objectAnimator
        android:duration="1000"
        android:propertyName="scaleX"
        android:valueFrom="1"
        android:valueTo="0.5" >
    </objectAnimator>
    <objectAnimator
        android:duration="1000"
        android:propertyName="scaleY"
        android:valueFrom="1"
        android:valueTo="0.5" >
    </objectAnimator>

</set>
```

使用
```
                Animator anim3 = AnimatorInflater.loadAnimator(this, R.animator.scale_anim);
                image.setPivotX(0);
                image.setPivotY(0);
                //显示的调用invalidate
                image.invalidate();
                anim3.setTarget(image);
                anim3.start();
```                
                




















