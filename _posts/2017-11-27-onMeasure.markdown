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














[onMeasure详解 - dmk877的专栏](http://blog.csdn.net/dmk877/article/details/49558367)

[Android开发之getMeasuredWidth和getWidth区别从源码分析 - dmk877的专栏](http://blog.csdn.net/dmk877/article/details/49734869)

[Android视图绘制流程完全解析，带你一步步深入了解View(二) - 郭霖的专栏](http://blog.csdn.net/guolin_blog/article/details/16330267)