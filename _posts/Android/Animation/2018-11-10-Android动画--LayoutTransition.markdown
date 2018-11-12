---
layout: post
title:  "Android动画--GridLayoutAnimation"
date:   2018-11-12 13:53:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 GridLayoutAnimation 』
---

> 针对`grideView`添加进入动画的`gridLayoutAnimation`。

### **简单使用**
```
// 动画文件 slide_in_left.xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" android:duration="1000">
    <translate android:fromXDelta="-50%p" android:toXDelta="0"/>
    <alpha android:fromAlpha="0.0" android:toAlpha="1.0" />
</set>

// Layout动画文件 gride_animation.xml
<?xml version="1.0" encoding="utf-8"?>
<gridLayoutAnimation xmlns:android="http://schemas.android.com/apk/res/android"
                     android:rowDelay="75%"
                     android:columnDelay="60%"
                     android:directionPriority="none"
                     android:animation="@anim/slide_in_left"/>
  
// Layout文件 main.xml   
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical">


    <Button
            android:id="@+id/add_data"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="添加grid数据"/>


    <GridView
            android:id="@+id/grid"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:columnWidth="60dp"
            android:gravity="center"
            android:horizontalSpacing="10dp"
            android:layoutAnimation="@anim/gride_animation"
            android:numColumns="auto_fit"
            android:stretchMode="columnWidth"
            android:verticalSpacing="10dp"/>

</LinearLayout>
```          

> 最重要的一点：`android:layoutAnimation`只在`viewGroup`创建的时候，才会对其中的`item`添加动画。在创建成功以后，再向其中添加item将不会再有动画。

### **gridLayoutAnimation各字段意义**

* `rowDelay`:每一行动画开始的延迟。与`LayoutAnimation`一样，可以取百分数，也可以取浮点数。取值意义为，当前`android:animation`所指动画时长的倍数。 

* `columnDelay`：每一列动画开始的延迟。取值类型及意义与rowDelay相同。 

* `directionPriority`：方向优先级。取值为`row`,`collumn`,`none`，意义分别为：行优先，列优先，和无优先级（同时进行）;具体意义，后面会细讲 

* `direction`：`gridview`动画方向。 取值有四个：

 . `left_to_right`：列，从左向右开始动画 
 
 . `right_to_left` ：列，从右向左开始动画 
 
 . `top_to_bottom`：行，从上向下开始动画 
 
 . `bottom_to_top`：行，从下向上开始动画 

 这四个值之间可以通过“|”连接，从而可以取多个值。很显然left_to_right和right_to_left是互斥的，top_to_bottom和bottom_to_top是互斥的。如果不指定 direction字段，默认值为left_to_right | top_to_bottom；即从上往下，从左往右。 

*  `animation`: gridview内部元素所使用的动画。

### **java实现**
```
        Animation animation = AnimationUtils.loadAnimation(MyActivity.this,R.anim.slide_in_left);
        GridLayoutAnimationController controller = new GridLayoutAnimationController(animation);
        controller.setColumnDelay(0.75f);
        controller.setRowDelay(0.5f);
        controller.setDirection(GridLayoutAnimationController.DIRECTION_BOTTOM_TO_TOP|GridLayoutAnimationController.DIRECTION_LEFT_TO_RIGHT);
        controller.setDirectionPriority(GridLayoutAnimationController.PRIORITY_NONE);
        grid.setLayoutAnimation(controller);
        grid.startLayoutAnimation();
```

参考：

[自定义控件三部曲之动画篇(十一)——layoutAnimation与gridLayoutAnimation - 启舰 - CSDN博客](https://blog.csdn.net/harvic880925/article/details/50785786)












