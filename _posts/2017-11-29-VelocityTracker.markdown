---
layout: post
title:  "VelocityTracker"
date:   2017-11-29 20:44:00 +0800
categories: Android
tags: 自定义ViewGroup
author: pepe
description: 自定义ViewGroup.
---
    
### [VelocityTracker][velocitytracker-url]

* `VelocityTracker`从字面意思理解那就是速度追踪器了，在滑动效果的开发中通常都是要使用该类计算出当前手势的初始速度.
* 对应的方法是`velocityTracker.computeCurrentVelocity(1000,mMaximumVelocity))`
* 并通过`getXVelocity`或`getYVelocity`方法得到对应的速度值`initialVelocity`
* 并将获得的速度值传递给Scroller类的`fling(int startX, int startY, int velocityX, int velocityY, int minX, int maxX, int minY, int maxY) `方法进行控件滚动时各种位置坐标数值的计算
* API中对fling 方法的解释是基于一个fling手势开始滑动动作,滑动的距离将由所获得的初始速度initialVelocity来决定。

#### 主要函数：
![VelocityTracker]({{ site.baseurl }}/assets/images/VelocityTracker.png)

#### 方法介绍
~~~
//获取一个VelocityTracker对象, 用完后记得回收    
//回收后代表你不需要使用了，系统将此对象在此分配到其他请求者    
static public VelocityTracker obtain();    
public void recycle(); 
    
//计算当前速度, 其中units是单位表示， 1代表px/毫秒, 1000代表px/秒, ..    
//maxVelocity此次计算速度你想要的最大值
//设置maxVelocity值为0.1时，速率大于0.01时，显示的速率都是0.01,速率小于0.01时，显示正常      
public void computeCurrentVelocity(int units, float maxVelocity); 

   
//经过一次computeCurrentVelocity后你就可以用一下几个方法获取此次计算的值    
//id是touch event触摸点的ID, 来为多点触控标识，有这个标识在计算时可以忽略    
//其他触点干扰，当然干扰肯定是有的    
public float getXVelocity();    
public float getYVelocity();    
public float getXVelocity(int id);    
public float getYVelocity(int id); 
~~~


#### [ViewConfiguration][ViewConfiguration-url]
主要使用了该类的下面三个方法:

* `configuration.getScaledTouchSlop()` //获得能够进行手势滑动的距离
* `configuration.getScaledMinimumFlingVelocity()`//获得允许执行一个fling手势动作的最小速度值
* `configuration.getScaledMaximumFlingVelocity()`//获得允许执行一个fling手势动作的最大速度值


参考：

[android VelocityTracker简单用法 - CSDN博客](http://blog.csdn.net/new_abc/article/details/46927399)

[自定义布局中的平滑移动 VelocityTracker()-速度追踪器的用法](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2012/1114/558.html)

[velocitytracker-url]:http://www.grepcode.com/file/repository.grepcode.com/java/ext/com.google.android/android/2.0_r1/android/view/VelocityTracker.java#VelocityTracker

[ViewConfiguration-url]:http://www.grepcode.com/file/repository.grepcode.com/java/ext/com.google.android/android/2.0_r1/android/view/ViewConfiguration.java#ViewConfiguration