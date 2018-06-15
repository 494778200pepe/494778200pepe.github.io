---
layout: post
title:  "Scroller实现简易版ViewPager2"
date:   2017-11-29 16:07:00 +0800
categories: Android
tags: 自定义ViewGroup
author: pepe
description: 『 将布局文件中的TextView换成Button(默认消费事件) 』
---

### `ScrollerLayout5.java`
~~~
public class ScrollerLayout5 extends ViewGroup {

    private int mScreenWidth;//屏幕宽度
    private int mScreenHeight;//屏幕高度
    private Scroller mScroller;
    private int mTouchSlop;
    private int leftBorder;
    private int rightBorder;

    public ScrollerLayout5(Context context, AttributeSet attrs) {
        super(context, attrs);
        mScreenWidth = ScreenUtils.getScreenWidth(context);
        mScreenHeight = ScreenUtils.getScreenHeight(context);
        mScroller = new Scroller(context);
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
            for (int i = 0; i < getChildCount(); i++) {
                View child = getChildAt(i);
                int childWidth = child.getMeasuredWidth();
                int childHeight = child.getMeasuredHeight();
                int childLeft = i * mScreenWidth + (mScreenWidth - childWidth) / 2;
                int childTop = (mScreenHeight - childHeight) / 2;
                int childRight = childLeft + childWidth;
                int childBottom = childTop + childHeight;
                child.layout(childLeft, childTop, childRight, childBottom);
            }
            leftBorder = 0;
            rightBorder = getChildCount() * mScreenWidth;
        }
    }

    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        Log.d("pepe", "===========> dispatchTouchEvent   +" + ev.getAction());
        return super.dispatchTouchEvent(ev);
    }

    private float mInterceptLastMoveX;
    private float mInterceptDownX;
    private float mInterceptMoveX;

    //disallowIntercept默认是false,0down,1up,2move,3cancel
    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        Log.d("pepe", "===========> onInterceptTouchEvent   +" + ev.getAction());
        switch (ev.getAction()) {
            case MotionEvent.ACTION_DOWN:
                Log.d("pepe", "onInterceptTouchEvent    MotionEvent.ACTION_DOWN");
                mInterceptDownX = ev.getRawX();
                mTouchInterceptMoveX = mInterceptLastMoveX = mInterceptDownX;
                Log.d("pepe", "onInterceptTouchEvent     mInterceptLastMoveX = " + mInterceptLastMoveX);
                break;
            case MotionEvent.ACTION_MOVE:
                Log.d("pepe", "onInterceptTouchEvent     MotionEvent.ACTION_MOVE");
                mInterceptMoveX = ev.getRawX();
                Log.d("pepe", "onInterceptTouchEvent     mInterceptMoveX = " + mInterceptMoveX);
                float diff = Math.abs(mInterceptMoveX - mInterceptLastMoveX);
                Log.d("pepe", "onInterceptTouchEvent     diff = " + diff);
                // 这里 mTouchInterceptMoveX 是否需要赋值？都可以
                // 如果赋值，那么从 DOWN 到这个 MOVE，之间的移动距离就损失了
                // 如果不赋值，那么 mTouchInterceptMoveX 直接就是 DOWN 时的值了，没有移动损失
                mTouchInterceptMoveX = mInterceptLastMoveX = mInterceptMoveX;
                Log.d("pepe", "onInterceptTouchEvent mLastMoveX = " + mInterceptLastMoveX);
                if (diff > mTouchSlop) {
                    Log.d("pepe", "===> onInterceptTouchEvent = true");
                    return true;
                }
                break;
            default:
                break;
        }
        boolean result = super.onInterceptTouchEvent(ev);
        // 默认都是不拦截的，所以Button能接受到事件，然后拦截，但是空白区域无法接收事件，也就不拦截了
        Log.d("pepe", "===========> onInterceptTouchEvent    result = " + result);
        return result;
    }
    // 这个变量是用于从 onInterceptTouchEvent 中获取 mInterceptLastMoveX 的值
    // 因为 Button 默认是消费事件的，所以onTouchEvent 是接收不到 MotionEvent.ACTION_DOWN  事件的， 所以mTouchLastMoveX是没有初始值的。
    private float mTouchInterceptMoveX;
    private float mTouchLastMoveX;
    private float mTouchDownX;
    private float mTouchMoveX;

    //『View控件』的「原始位置信息」
    //『View控件』:View.getWidth()/View.getHeight()。View.getWidth() = getRight()-getLeft()
    //『View控件』:View.getTop()/View.getRight()/View.getBottom()/View.getLeft()

    //『View控件』的「实时位置信息和偏移值」
    //getTranslationX():计算的是该『View控件』在X轴的偏移量。初始值为0，向左偏移值为负，向右偏移值为正。
    //getTranslationY():计算的是该『View控件』在Y轴的偏移量。初始值为0，向上偏移为负，向下偏移为证。
    // 可以通过执行属性动画来使偏移。
    //View.getX():子View相对于父View在X轴上的实时偏移值。getX() = getLeft() + getTranslationX();
    //View.getY():子View相对于父View在y轴上的实时偏移值。getY() = getTop() + getTranslationY();

    //『View内容』的 「位置信息」
    //mScrollX 的值总等于 『View控件左边缘』 和 『View内容左边缘』 在水平方向上的距离。
    //mScrollY 的值总等于 『View控件上边缘』 和 『View内容上边缘』 在垂直方向上的距离。
    //Android并没有提供『View内容』的坐标信息接口，所以View.getScrollX()间接的表示了『View内容』的坐标信息。
    //记住  滑动时，滑动的是『View内容』，而不是『View控件』，所以 下面的 event.getX()  。。。

    //『触摸点』相对 「屏幕」
    //event.getRawX:表示的是触摸点距离 屏幕左边界的距离;
    // event.getRawY:表示的是触摸点距离 屏幕上边界的距离
    //『触摸点』相对 「View控件」
    //event.getX():表示的是触摸的点距离 自身左边界的距离;event.getY():表示的是触摸的点距离 自身上边界的距离
    //event.getX()  相对的是『View控件』，滑动时『View控件』不动，所以 event.getX() = event.getRawX
    //如果用属性动画，改变了『View控件』 的「实时位置信息和偏移值」，那么event.getX()和event.getRawX()就不相同了。

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        Log.d("pepe", "===========> onTouchEvent   +" + event.getAction());
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                Log.d("pepe", "onTouchEvent    MotionEvent.ACTION_DOWN");
                // 如果是触摸 Button，button 就会拦截了，这里就不会执行
                // 如果是空白部分，就不会拦截
                mTouchDownX = event.getRawX();
                mTouchLastMoveX = mTouchDownX;
                Log.d("pepe", "onTouchEvent     mTouchLastMoveX = " + mTouchLastMoveX);
                break;
            case MotionEvent.ACTION_MOVE:
                Log.d("pepe", "onTouchEvent    MotionEvent.ACTION_MOVE");
                Log.d("pepe", "onTouchEvent     mTouchInterceptMoveX = " + mTouchInterceptMoveX);
                mTouchMoveX = event.getRawX();
                Log.d("pepe", "onTouchEvent     mTouchMoveX = " + mTouchMoveX);
                // 从左向右滑动，getScrollX()需要减，mLastMoveX < mTouchMoveX，scrolledX正好为负值
                // 从右向左滑动，getScrollX()需要加，mLastMoveX > mTouchMoveX，scrolledX正好为正值
                // scrolledX 直接和 getScrollX() 做加法就好了
                int scrolledX = (int) (mTouchInterceptMoveX - mTouchMoveX);
                Log.d("pepe", "onTouchEvent     scrolledX = " + scrolledX);
                Log.d("pepe", "onTouchEvent     getScrollX() = " + getScrollX());
                Log.d("pepe", "onTouchEvent     getScrollX() + scrolledX = " + (getScrollX() + scrolledX));
                Log.d("pepe", "onTouchEvent     getScrollX() + getWidth() + scrolledX = " + (getScrollX() + getWidth() + scrolledX));
                if (getScrollX() + scrolledX < leftBorder) {
                    scrollTo(leftBorder, 0);
                    return true;
                } else if (getScrollX() + scrolledX + getWidth() > rightBorder) {
                    scrollTo(rightBorder - getWidth(), 0);
                    return true;
                }
                scrollBy(scrolledX, 0);
                mTouchInterceptMoveX = mTouchMoveX;
                break;
            case MotionEvent.ACTION_UP:
                int targetIndex = (getScrollX() + getWidth() / 2) / getWidth();
                int dx = targetIndex * getWidth() - getScrollX();
                mScroller.startScroll(getScrollX(), 0, dx, 0);
                invalidate();
                break;
            default:
                break;
        }
        //return super.onTouchEvent(event);//如果MyViewGroup里面放的是Button，这样可以
        //如果是TextView等，就必须返回true，以消费事件，否则后续事件不会传过来的
        return true;
    }

    @Override
    public void computeScroll() {
        if (mScroller.computeScrollOffset()) {
            scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
            invalidate();
        }
    }
}
~~~

### `layout5.xml`
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:focusable="true"
    android:background="@android:color/holo_green_dark">

    <com.hopechart.widget.ScrollerLayout1
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <Button
            android:id="@+id/btn1"
            android:layout_width="match_parent"
            android:layout_marginTop="200dp"
            android:layout_height="500dp"
            android:background="@android:color/white"
            android:onClick="onClick"
            android:text="page1"
            android:textColor="@color/main_color" />

        <Button
            android:id="@+id/btn2"
            android:layout_width="match_parent"
            android:layout_marginTop="200dp"
            android:layout_height="500dp"
            android:background="@android:color/white"
            android:onClick="onClick"
            android:text="page2"
            android:textColor="@color/main_color" />

        <Button
            android:id="@+id/btn3"
            android:layout_width="match_parent"
            android:layout_marginTop="200dp"
            android:layout_height="500dp"
            android:background="@android:color/white"
            android:onClick="onClick"
            android:text="page3"
            android:textColor="@color/main_color" />


    </com.hopechart.widget.ScrollerLayout1>
</RelativeLayout>
```



参考：

[Android Scroller完全解析，关于Scroller你所需知道的一切](http://blog.csdn.net/guolin_blog/article/details/48719871)



