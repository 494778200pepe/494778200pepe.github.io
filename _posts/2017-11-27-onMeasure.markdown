---
layout: post
title:  "onMeasure"
date:   2017-11-27 10:58:00 +0800
categories: Android
tags: 自定义ViewGroup
author: pepe
description: 自定义ViewGroup.
---

做学问，容不得半点马虎和偷懒!
============

measure过程都干了点什么事？
 由于在用自适应尺寸来定义View大小的时候，View的真实尺寸还不能确定。但是View尺寸最终需要映射到屏幕上的像素大小，所以measure过程就是干这件事，把各种尺寸值，经过计算，得到具体的像素值。measure过程会遍历整棵View树，然后依次测量每个View真实的尺寸。具体是每个ViewGroup会向它内部的每个子View发送measure命令，然后由具体子View的onMeasure()来测量自己的尺寸。最后测量的结果保存在View的mMeasuredWidth和mMeasuredHeight中，保存的数据单位是像素。


# 常用方法

~~~
    int widthMode = MeasureSpec.getMode(widthMeasureSpec);  
    int heightMode = MeasureSpec.getMode(heightMeasureSpec);  
    int sizeWidth = MeasureSpec.getSize(widthMeasureSpec);  
    int sizeHeight = MeasureSpec.getSize(heightMeasureSpec); 
~~~
获得此ViewGroup上级容器为其推荐的宽和高，以及计算模式 	
~~~
    measureChildren(widthMeasureSpec, heightMeasureSpec);  
    View childView = getChildAt(i);  
    cWidth = childView.getMeasuredWidth();  
    cHeight = childView.getMeasuredHeight();  
    cParams = (MarginLayoutParams) childView.getLayoutParams(); 
~~~
计算出所有的childView的宽和高  	
~~~	
    setMeasuredDimension(measuredWidth, measuredHeight);
~~~
设置ViewGroup的宽和高	






参考：

[Android自定义View（三、深入解析控件测量onMeasure）](http://blog.csdn.net/xmxkf/article/details/51490283)

[Android View框架的measure机制 - 黄金夫 - 博客园](http://www.cnblogs.com/xyhuangjinfu/p/5435201.html)

[Android onMeasure、Measure、measureChild、measureChildren 一些简要说明](http://blog.csdn.net/jjwwmlp456/article/details/43964785)

[onMeasure详解 - dmk877的专栏](http://blog.csdn.net/dmk877/article/details/49558367)

[Android开发之getMeasuredWidth和getWidth区别从源码分析 - dmk877的专栏](http://blog.csdn.net/dmk877/article/details/49734869)

[Android视图绘制流程完全解析，带你一步步深入了解View(二) - 郭霖的专栏](http://blog.csdn.net/guolin_blog/article/details/16330267)