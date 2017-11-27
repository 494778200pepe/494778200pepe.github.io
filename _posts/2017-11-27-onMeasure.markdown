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

# 测量流程

	先定义一个结构：ViewGroup1 包含  ViewGroup2 和一个 TextView1
	
    ViewGroup2 包含 TextView2 和 TextView3
	那么测量的过程是：
~~~
        ViewGroup1.measure -> ViewGroup1.onMeasure 然后循环遍历子View，先ViewGroup2，接着TextView1，
            //也就是在measureChildren()中调用measureChild()
            ViewGroup2.measure -> ViewGroup2.onMeasure 然后循环遍历子View，TextView2，TextView3
                    TextView2.measure -> TextView2.onMeasure -> TextView2.setMeasuredDimension
                    TextView3.measure -> TextView3.onMeasure -> TextView3.setMeasuredDimension
        		ViewGroup2.setMeasuredDimension
			TextView1.measure -> TextView1.onMeasure
				TextView1.setMeasuredDimension
		ViewGroup1.setMeasuredDimension
~~~
	测量之后是保存，调用setMeasuredDimension()保存在 mMeasuredWidth 和 mMeasuredHeight 里面。

# 关键方法
   
~~~   
    /**
    *遍历ViewGroup中所有的子控件，调用measuireChild测量宽高
    */
    protected void measureChildren (int widthMeasureSpec, int heightMeasureSpec) {
        final int size = mChildrenCount;
        final View[] children = mChildren;
        for (int i = 0; i < size; ++i) {
            final View child = children[i];
            if ((child.mViewFlags & VISIBILITY_MASK) != GONE) {
                //测量某一个子控件宽高
                measureChild(child, widthMeasureSpec, heightMeasureSpec);
            }
        }
    }
    
    /**
     * 测量某一个child的宽高
    */
    protected void measureChild (View child, int parentWidthMeasureSpec,
        int parentHeightMeasureSpec) {
        final LayoutParams lp = child.getLayoutParams();
        //获取子控件的宽高约束规则
        final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
            mPaddingLeft + mPaddingRight, lp. width);
        final int childHeightMeasureSpec = getChildMeasureSpec(parentHeightMeasureSpec,
            mPaddingTop + mPaddingBottom, lp. height);

        child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
    }

    /**
     * 测量某一个child的宽高，考虑margin值
    */
    protected void measureChildWithMargins (View child,
        int parentWidthMeasureSpec, int widthUsed,
        int parentHeightMeasureSpec, int heightUsed) {
        final MarginLayoutParams lp = (MarginLayoutParams) child.getLayoutParams();
        //获取子控件的宽高约束规则
        final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
            mPaddingLeft + mPaddingRight + lp. leftMargin + lp.rightMargin
                   + widthUsed, lp. width);
        final int childHeightMeasureSpec = getChildMeasureSpec(parentHeightMeasureSpec,
            mPaddingTop + mPaddingBottom + lp. topMargin + lp.bottomMargin
                   + heightUsed, lp. height);
        //测量子控件
        child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
    }
~~~

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