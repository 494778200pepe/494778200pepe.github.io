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


参考：


[滑动速度跟踪类VelocityTracker介绍](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2012/1117/574.html)

[android VelocityTracker简单用法 - CSDN博客](http://blog.csdn.net/new_abc/article/details/46927399)

[velocitytracker-url]:http://www.grepcode.com/file/repository.grepcode.com/java/ext/com.google.android/android/2.0_r1/android/view/VelocityTracker.java#VelocityTracker

