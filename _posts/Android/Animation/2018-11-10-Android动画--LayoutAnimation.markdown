---
layout: post
title:  "Android动画--LayoutAnimation"
date:   2018-11-12 13:36:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 LayoutAnimation 』
---

### **简单使用**
```
// 动画文件 layout_alpha.xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="@android:anim/accelerate_interpolator"
    android:shareInterpolator="true"
    >
    <alpha
        android:fromAlpha="0"
        android:toAlpha="1"
        android:duration="3000"
        />
</set>

// Layout动画文件 layoutanim.xml
<?xml version="1.0" encoding="utf-8"?>
<layoutAnimation
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:delay="0.5"
    android:animationOrder="normal"
    android:animation="@anim/layout_alpha"
    />
   
// Layout文件 act_layout_anim.xml   
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/id_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:animateLayoutChanges="true"
    android:layoutAnimation="@anim/layoutanim">

    ...

</LinearLayout>
```          




















