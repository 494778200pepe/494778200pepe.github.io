---
layout: post
title:  "Scroller实现简易版ViewPager"
date:   2017-11-29 15:07:00 +0800
categories: Android
tags: 自定义ViewGroup
author: pepe
description: 『 Scroller实现简易版ViewPager 』
---

### `ScrollerLayout.java`
~~~
import android.content.Context;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;
import android.view.ViewConfiguration;
import android.view.ViewGroup;
import android.widget.Scroller;

/**
 * Created by wang on 2017/7/17.
 */

public class ScrollerLayout extends ViewGroup {

    private Scroller mScroller;
    private int mTouchSlop;
    private float mXDown;
    private float mXMove;
    private float mXlastMove;
    private int leftBorder;
    private int rightBorder;

    public ScrollerLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        ViewConfiguration configuration = ViewConfiguration.get(context);
        mTouchSlop = configuration.getScaledTouchSlop();
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
//        measureChildren(widthMeasureSpec, heightMeasureSpec);
//        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int childCount = getChildCount();
        for (int i = 0; i < childCount; i++) {
            View childView = getChildAt(i);
            measureChild(childView, widthMeasureSpec, heightMeasureSpec);
        }
    }

    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        if (changed) {
            int childCount = getChildCount();
            for (int i = 0; i < childCount; i++) {
                View childView = getChildAt(i);
                childView.layout(i * childView.getMeasuredWidth(), 0, (i + 1) * childView.getMeasuredWidth(), childView.getMeasuredHeight());
            }
        }
        leftBorder = getChildAt(0).getLeft();
        rightBorder = getChildAt(getChildCount() - 1).getRight();
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        switch (ev.getAction()) {
            case MotionEvent.ACTION_DOWN:
                //event.getRawX:表示的是触摸点距离 屏幕左边界的距离
                //event.getRawY:表示的是触摸点距离 屏幕上边界的距离
                mXDown = ev.getRawX();
                mXlastMove = mXDown;
                break;
            case MotionEvent.ACTION_MOVE:
                mXMove = ev.getRawX();
                float diff = mXMove - mXlastMove;
                mXlastMove = mXMove;
                if (diff > mTouchSlop) {
                    return true;
                }
                break;
        }
        return super.onInterceptTouchEvent(ev);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                break;
            case MotionEvent.ACTION_MOVE:
                //event.getRawX:表示的是触摸点距离 屏幕左边界的距离
                //event.getRawY:表示的是触摸点距离 屏幕上边界的距离
                mXMove = event.getRawX();
                int scrolledX = (int) (mXlastMove - mXMove);
                if (getScrollX() + scrolledX < leftBorder) {
                    scrollTo(leftBorder, 0);
                    return true;
                } else if (getScrollX() + scrolledX + getWidth() > rightBorder) {
                    scrollTo(rightBorder - getWidth(), 0);
                    return true;
                }
                scrollBy(scrolledX, 0);
                mXlastMove = mXMove;
                break;
            case MotionEvent.ACTION_UP:
                int targetIndex = (getScrollX() + getWidth() / 2) / getWidth();
                int dx = targetIndex * getWidth() - getScrollX();
                mScroller.startScroll(getScrollX(),0,dx,0);
                invalidate();
                break;
        }
        //return super.onTouchEvent(event);//如果MyViewGroup里面放的是Button，这样可以
        return true;//如果是TextView等，就必须返回true，以消费事件，否则后续事件不会传过来的
    }

    @Override
    public void computeScroll() {
        if(mScroller.computeScrollOffset()){
            scrollTo(mScroller.getCurrX(),mScroller.getCurrY());
            invalidate();
        }
    }
}
~~~

### `activity_main.xml`
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:focusable="true"
    android:background="@android:color/holo_green_dark">

    <com.hopechart.widget.MyViewGroup
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:layout_width="100dp"
            android:layout_height="wrap_content"
            android:background="@android:color/white"
            android:text="page1"
            android:textColor="@color/main_color" />

        <TextView
            android:layout_width="200dp"
            android:layout_height="300dp"
            android:background="@android:color/white"
            android:text="page2"
            android:textColor="@color/main_color" />

        <TextView
            android:layout_width="300dp"
            android:layout_height="200dp"
            android:background="@android:color/white"
            android:text="page3"
            android:textColor="@color/main_color" />

    </com.hopechart.widget.MyViewGroup>
</RelativeLayout>
```



参考：

[Android Scroller完全解析，关于Scroller你所需知道的一切](http://blog.csdn.net/guolin_blog/article/details/48719871)



