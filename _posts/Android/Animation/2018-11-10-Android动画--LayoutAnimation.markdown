---
layout: post
title:  "Android动画--LayoutAnimation"
date:   2018-11-12 13:36:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 LayoutAnimation 』
---

> 普通`viewGroup`添加进入统一动画的`LayoutAnimation`，在API 1中就有。

### **简单使用**
```
// 动画文件 layout_alpha.xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="@android:anim/accelerate_interpolator"
    android:shareInterpolator="true">
    <translate
        android:fromXDelta="-50%p"
        android:toXDelta="0" />
    <alpha
        android:duration="3000"
        android:fromAlpha="0"
        android:toAlpha="1" />
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

> 最重要的一点：`android:layoutAnimation`只在`viewGroup`创建的时候，才会对其中的`item`添加动画。在创建成功以后，再向其中添加item将不会再有动画。

### **layoutAnimation各字段意义**

*   `delay:指每个Item的动画开始延时`，取值是`android:animation`所指定动画时长的倍数，取值类型可以是float类型，也可以是百分数，默认是`0.5`;比如我们这里指定的动画是`@anim/slide_in_left`，而在`slide_in_left.xml`中指定`android:duration=”1000”`，即单次动画的时长是1000毫秒，而我们在这里的指定`android:delay=”1”`，即一个Item的动画会在上一个item动画完成后延时单次动画时长的一倍时间开始，即延时1000毫秒后开始。

* `animationOrder`:指`viewGroup`中的控件动画开始顺序，取值有`normal(正序)`、`reverse(倒序)`、`random(随机)`

* `animation`：指定每个item入场所要应用的动画。仅能指定`res/aim`文件夹下的`animation`定义的动画，不可使用animator动画。

### **java实现**
```
Animation animation= AnimationUtils.loadAnimation(this,R.anim.slide_in_left);   
//得到一个LayoutAnimationController对象；
LayoutAnimationController controller = new LayoutAnimationController(animation);   //设置控件显示的顺序；
controller.setOrder(LayoutAnimationController.ORDER_REVERSE);  
//设置控件显示间隔时间；
controller.setDelay(0.3f);   
//为ListView设置LayoutAnimationController属性；
mListView.setLayoutAnimation(controller);
mListView.startLayoutAnimation();
```

参考：

[自定义控件三部曲之动画篇(十一)——layoutAnimation与gridLayoutAnimation - 启舰 - CSDN博客](https://blog.csdn.net/harvic880925/article/details/50785786)












