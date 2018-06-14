---
layout: post
title:  "Scroller实现VelocityTracker版ViewPager"
date:   2017-12-01 21:44:00 +0800
categories: Android
tags: 自定义ViewGroup
author: pepe
description: 『 Scroller实现VelocityTracker版ViewPager 』
---

### `ScrollerLayout.java`
~~~
import android.content.Context;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.view.VelocityTracker;
import android.view.View;
import android.view.ViewConfiguration;
import android.view.ViewGroup;
import android.widget.Scroller;

/**
 * @author wang
 * @date 2018/6/12.
 */

public class ScrollerLayout2 extends ViewGroup {

    public static final String TAG = "pepe";
    /**
     * 屏幕宽度
     */
    private int mScreenWidth;
    private int mScreenHeight;
    /**
     * 用于完成滚动操作的实例
     */
    private Scroller mScroller;

    /**
     * 判定为拖动的最小移动像素数
     */
    private int mTouchSlop;

    /**
     * 手机按下时的屏幕坐标
     */
    private float mXDown;

    /**
     * 手机当时所处的屏幕坐标
     */
    private float mXMove;

    /**
     * 上次触发ACTION_MOVE事件时的屏幕坐标
     */
    private float mXLastMove;

    /**
     * 界面可滚动的左边界
     */
    private int leftBorder;
    /**
     * 界面可滚动的右边界
     */
    private int rightBorder;
    /**
     * getScaledPagingTouchSlop
     */
    private VelocityTracker mVelocityTracker;
    
    private int mMinimumVelocity;
    private int mMaximumVelocity;

    public ScrollerLayout2(Context context, AttributeSet attrs) {
        super(context, attrs);
        mScreenWidth = ScreenUtils.getScreenWidth(context);
        mScreenHeight = ScreenUtils.getScreenHeight(context);
        mScroller = new Scroller(context);
        ViewConfiguration viewConfiguration = ViewConfiguration.get(context);
        // 获取TouchSlop值
        mTouchSlop = viewConfiguration.getScaledTouchSlop();
        Log.d("pepe", "mTouchSlop = " + mTouchSlop);
        mMinimumVelocity = viewConfiguration.getScaledMinimumFlingVelocity();
        mMaximumVelocity = viewConfiguration.getScaledMaximumFlingVelocity();
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        // 计算出所有的childView的宽和高
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
    protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
        Log.d("pepe", "onLayout     getChildCount = " + getChildCount());
        if (changed) {
            for (int i = 0; i < getChildCount(); i++) {
                View child = getChildAt(i);
                int childWidth = child.getMeasuredWidth();
                int childHeight = child.getMeasuredHeight();
                Log.d("pepe", "childWidth = " + childWidth);
                Log.d("pepe", "childHeight = " + childHeight);
                int childLeft = i * mScreenWidth + (mScreenWidth - childWidth) / 2;
                int childTop = (mScreenHeight - childHeight) / 2;
                int childRight = childLeft + childWidth;
                int childBottom = childTop + childHeight;
                child.layout(childLeft, childTop, childRight, childBottom);
            }
            leftBorder = 0;
            rightBorder = getChildCount() * mScreenWidth;
            Log.d("pepe", "leftBorder = " + leftBorder);
            Log.d("pepe", "rightBorder = " + rightBorder);
        }

    }

    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        return super.dispatchTouchEvent(ev);
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        Log.d("pepe", "onInterceptTouchEvent");
        switch (ev.getAction()) {
            case MotionEvent.ACTION_DOWN:
                Log.d("pepe", "MotionEvent.ACTION_DOWN");
                mXDown = ev.getRawX();
                mXLastMove = mXDown;
                break;
            case MotionEvent.ACTION_MOVE:
                Log.d("pepe", "MotionEvent.ACTION_MOVE");
                mXMove = ev.getRawX();
                float diff = Math.abs(mXMove - mXDown);
                Log.d("pepe", "diff = " + diff);
                mXLastMove = mXMove;
                // 当手指拖动值大于TouchSlop值时，认为应该进行滚动，拦截子控件的事件
                if (diff > mTouchSlop) {
                    Log.d("pepe", "onInterceptTouchEvent = true");
                    return true;
                }
                break;
            default:
                break;
        }
        return super.onInterceptTouchEvent(ev);
    }

    //event.getRawX:表示的是触摸点距离 屏幕左边界的距离;event.getRawY:表示的是触摸点距离 屏幕上边界的距离
    //event.getX():表示的是触摸的点距离 自身左边界的距离;event.getY():表示的是触摸的点距离 自身上边界的距离
    
    //mScrollX 的值总等于 『View控件左边缘』 和 『View内容左边缘』 在水平方向上的距离。
    //mScrollY 的值总等于 『View控件上边缘』 和 『View内容上边缘』 在垂直方向上的距离
    // getScrollX() = getLeft() - getX();
    // getScrollY() = getTop() - getY();
    //『View控件』:View.getWidth()/View.getHeight()。View.getWidth() = getRight()-getLeft()
    //『View控件』:View.getTop()/View.getRight()/View.getBottom()/View.getLeft()
    //『View内容』:View.getX()/View.getY()，相对于ViewGroup
    //『View内容』:View.getTranslationX()/getTranslationY.getY()，相对于自身
    //View.getTranslationX()计算的是该View在X轴的偏移量。初始值为0，向左偏移值为负，向右偏移值为正。 = newX - oldX
    //View.getTranslationY()计算的是该View在Y轴的偏移量。初始值为0，向上偏移为负，向下偏移为证。 = newY = oldY
    // getX() = getLeft() + getTranslationX();
    // getY() = getTop() + getTranslationY();
    // getScrollX() = getLeft() - (getLeft() + getTranslationX()) = - getTranslationX(); 向右为负
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        Log.d("pepe", "onTouchEvent");
        obtainVelocityTracker(event);
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                break;
            case MotionEvent.ACTION_MOVE:
                Log.d("pepe", "onTouchEvent     MotionEvent.ACTION_MOVE");
                mXMove = event.getRawX();
                int scrolledX = (int) (mXLastMove - mXMove);
                Log.d("pepe", "scrolledX = " + scrolledX);
                Log.d("pepe", "getScrollX() = " + getScrollX());
                Log.d("pepe", "getScrollX() + scrolledX = " + (getScrollX() + scrolledX));
                Log.d("pepe", "getScrollX() + getWidth() + scrolledX = " + (getScrollX() + getWidth() + scrolledX));
                if (getScrollX() + scrolledX < leftBorder) {
                    scrollTo(leftBorder, 0);
                    return true;
                } else if (getScrollX() + getWidth() + scrolledX > rightBorder) {
                    scrollTo(rightBorder - getWidth(), 0);
                    return true;
                }
                scrollBy(scrolledX, 0);
                mXLastMove = mXMove;
                break;
            case MotionEvent.ACTION_UP:
                Log.d("pepe", "onTouchEvent     MotionEvent.ACTION_UP");
                mVelocityTracker.computeCurrentVelocity(1000, mMaximumVelocity);
                int initialVelocity = (int) mVelocityTracker.getXVelocity();
                if ((Math.abs(initialVelocity) > mMinimumVelocity)
                        && getChildCount() > 0) {
                    isFling = true;
                    fling(-initialVelocity);
                } else {
                    // 当手指抬起时，根据当前的滚动值来判定应该滚动到哪个子控件的界面
                    int targetIndex = (getScrollX() + getWidth() / 2) / getWidth();
                    int dx = targetIndex * getWidth() - getScrollX();
                    // 第二步，调用startScroll()方法来初始化滚动数据并刷新界面
                    mScroller.startScroll(getScrollX(), 0, dx, 0);
                    invalidate();
                }
                releaseVelocityTracker();
                break;
            default:
                break;
        }
        return true;
    }

    boolean isFling = false;
    @Override
    public void computeScroll() {
        // 第三步，重写computeScroll()方法，并在其内部完成平滑滚动的逻辑
        if (mScroller.computeScrollOffset()) {
            scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
            if(isFling){
                //这是最后OverScroller的最后一次滑动，如果这次滑动完了mCurrentScale不是整数，则把尺子移动到最近的整数位置
                if (!mScroller.computeScrollOffset()) {
                    isFling = false;
                    int targetIndex = (getScrollX() + getWidth() / 2) / getWidth();
                    int dx = targetIndex * getWidth() - getScrollX();
                    // 第二步，调用startScroll()方法来初始化滚动数据并刷新界面
                    mScroller.startScroll(getScrollX(), 0, dx, 0);
                    invalidate();
                }
            }
            invalidate();
        }else{
            isFling = false;
        }
    }

    public void fling(int velocityX) {
        Log.d("pepe", "fling");
        if (getChildCount() > 0) {
            Log.d("pepe", "pepe     getScrollX() = " + getScrollX());
            Log.d("pepe", "pepe     velocityX = " + velocityX);
            mScroller.fling(getScrollX(), getScrollY(), velocityX, 0, leftBorder, rightBorder - getWidth(), 0,
                    0);
            awakenScrollBars(mScroller.getDuration());
            invalidate();
        }
    }

    private void obtainVelocityTracker(MotionEvent event) {
        if (mVelocityTracker == null) {
            mVelocityTracker = VelocityTracker.obtain();
        }
        mVelocityTracker.addMovement(event);
    }


    private void releaseVelocityTracker() {
        if (mVelocityTracker != null) {
            mVelocityTracker.recycle();
            mVelocityTracker = null;
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

    <com.hopechart.widget.ScrollerLayout2
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

    </com.hopechart.widget.ScrollerLayout2>
</RelativeLayout>
```



参考：

[Android Scroller完全解析，关于Scroller你所需知道的一切](http://blog.csdn.net/guolin_blog/article/details/48719871)



