---
layout: post
title:  "onMeasure"
date:   2017-11-27 10:58:00 +0800
categories: Android
tags: 自定义ViewGroup
author: pepe
description: 自定义ViewGroup.
---


### 测量流程
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

### 关键方法
   
#### ViewGroup:
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

注意：
    这里的 widthMeasureSpec 和 heightMeasureSpec 从哪里来的？生成的，根据 ViewGroup 的 LayoutParams来的，最顶层一定是 MatchParent。


#### View:

~~~
    protected void onMeasure( int widthMeasureSpec, int heightMeasureSpec) {
        setMeasuredDimension( getDefaultSize(getSuggestedMinimumWidth(), widthMeasureSpec),
            getDefaultSize(getSuggestedMinimumHeight(), heightMeasureSpec));
    }
    
    /**
     * 为宽度获取一个建议最小值
    */
    protected int getSuggestedMinimumWidth () {
        return (mBackground == null) ? mMinWidth : max(mMinWidth , mBackground.getMinimumWidth());
    }

    /**
     * 获取默认的宽高值
    */
    public static int getDefaultSize (int size, int measureSpec) {
        int result = size;
        int specMode = MeasureSpec. getMode(measureSpec);
        int specSize = MeasureSpec. getSize(measureSpec);
        switch (specMode) {
            case MeasureSpec. UNSPECIFIED:
                result = size;
                break;
            case MeasureSpec. AT_MOST:
            case MeasureSpec. EXACTLY:
                result = specSize;
                break;
        }   
        return result;
    }
~~~
注意：
    1 "子View的MeasureSpec由父容器的MeasureSpec和自身的LayoutParams共同决定"
    2 onMeasure方法最后需要调用setMeasuredDimension方法来保存测量的宽高值，如果不调用这个方法，可能会产生不可预测的问题。


#### getChildMeasureSpec():      

    父                   子                               子控件的约束规则                 
    EXACTLY         具体的size（20dip）/MATCH_PARENT         EXACTLY	
    
说明：子控件如果是具体值，约束尺寸就是这个值，模式为确定的；子控件为填充父窗体，约束尺寸是父控件剩余大小，模式为确定的。
    
                    WRAP-CONTENT                             AT_MOST
                    
说明：子控件如果是包裹内容，约束尺寸值为父控件剩余大小 ，模式为至多   
                 
    AT_MOST         具体的size（20dip）/MATCH_PARENT         EXACTLY
    
说明：子控件如果是具体值，约束尺寸就是这个值，模式为确定的；

                    WRAP-CONTENT                             AT_MOST
                    
说明：子控件为填充父窗体或者包裹内容 ，约束尺寸是父控件剩余大小 ，模式为至多

    UNSPECIFIED     具体的size（20dip）                      EXACTLY
    
说明：子控件如果是具体值，约束尺寸就是这个值，模式为确定的   

                    MATCH_PARENT/WRAP_CONTENT                UNSPECIFIED
                    
说明：子控件为填充父窗体或者包裹内容 ，约束尺寸0，模式为未指定   
                 
   
#### MeasureSpec 
网上介绍的资料很多 
   
onMeasure方法测量控件大小的流程，以及里面执行的一些细节，总结一下知识点：

    1、 测量控件大小是父控件发起的
    2、 父控件要测量子控件大小，需要重写onMeasure方法，然后调用measureChildren或者measureChildWithMargin方法
    3、 on Measure方法的参数是通过getChildMeasureSpec生成的
    4、 如果我们自定义控件需要使用wrap_content，我们需要重写onMeasure方法
    5、 测量控件的步骤：
        1、 父控件onMeasure->measureChildren`measureChildWithMargin->getChildMeasureSpec->
        2、 子控件的measure->onMeasure->setMeasureDimension->
        3、 父控件onMeasure结束调用setMeasureDimension`保存自己的大小
   
   
   

#### 参考：

[Android自定义View（三、深入解析控件测量onMeasure）](http://blog.csdn.net/xmxkf/article/details/51490283)

[Android View框架的measure机制 - 黄金夫 - 博客园](http://www.cnblogs.com/xyhuangjinfu/p/5435201.html)

[Android onMeasure、Measure、measureChild、measureChildren 一些简要说明](http://blog.csdn.net/jjwwmlp456/article/details/43964785)

[onMeasure详解 - dmk877的专栏](http://blog.csdn.net/dmk877/article/details/49558367)

[Android开发之getMeasuredWidth和getWidth区别从源码分析 - dmk877的专栏](http://blog.csdn.net/dmk877/article/details/49734869)

[Android视图绘制流程完全解析，带你一步步深入了解View(二) - 郭霖的专栏](http://blog.csdn.net/guolin_blog/article/details/16330267)