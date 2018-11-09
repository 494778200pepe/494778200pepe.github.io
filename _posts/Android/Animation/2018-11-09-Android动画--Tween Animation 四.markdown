---
layout: post
title:  "Android动画--Tween Animation 四"
date:   2018-11-09 11:38:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 translate 』
---

### **xml实现**
```
<?xml version="1.0" encoding="utf-8"?>
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="3000"
    android:fillBefore="true"
    android:fromXDelta="0"
    android:fromYDelta="0"
    android:toXDelta="200"
    android:toYDelta="150" />
```

属性说明：
```
public TranslateAnimation(int fromXType, float fromXValue, int toXType, float toXValue, int fromYType, float fromYValue, int toYType, float toYValue) {
        throw new RuntimeException("Stub!");
}

可以设置四个起始结束点的type，默认mPivotXType = ABSOLUTE;两个点四个type
```

### **开始动画**
```
Animation aa3 = AnimationUtils.loadAnimation(this, R.anim.translateanim);
image.startAnimation(aa3);
```

### **Java构造**
```
TranslateAnimation aa1 = new TranslateAnimation(0,0,1,0.5f,0,0,1,0.5f);
aa1.setDuration(3000);
image.startAnimation(aa1);
```                




























