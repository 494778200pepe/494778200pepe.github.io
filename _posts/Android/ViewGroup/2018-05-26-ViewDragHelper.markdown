---
layout: post
title:  "ViewDragHelper"
date:   2018-05-26 14:31:00 +0800
categories: Android
tags: 自定义ViewGroup
author: pepe
description: 『 ViewDragHelper 』
---

    一、自定义ViewGroup里通过ViewDragHelper静态工厂方法create()创建实例并实现ViewDragHelper.CallBack抽象类。

    二、在自定义ViewGroup的onInterceptTouchEvent()方法里调用并返回ViewDragHelper的shouldInterceptTouchEvent()方法，
        在onTouchEvent()方法里调用ViewDragHelper()的processTouchEvent()方法，且返回true（因为ACTION_DOWN时如果子View没有消费事件，
        我们需要在onTouchEvent()中返回true，否则收不到后续的事件，从而不会产生拖动等效果）。

    三、依据自己需求实现ViewDragHelper.CallBack中相关方法即可。

    四、至此已经实现了子View拖动效果，如果需要Fling或者惯性滚动效果则还需要实现自定义ViewGroup的computeScroll()方法进行手动刷帧。




1、 创建：mDragHelper = ViewDragHelper.create(this, mDragHelperCallback);
2、 TouchSlop：
    系统能识别出被认为是滑动的最小距离，小于这个常量，系统不认为你在进行滑动。与设备有关。
    通过
        ViewConfigration.get(getContext()).getScaledTouchSlop()
    方法获得
3、 @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        final int action = MotionEventCompat.getActionMasked(ev);
        if (action == MotionEvent.ACTION_CANCEL || action == MotionEvent.ACTION_UP) {
            mDragHelper.cancel();
            return false;
        }
        return mDragHelper.shouldInterceptTouchEvent(ev);
    }
4、 @Override
    public boolean onTouchEvent(MotionEvent ev) {
        mDragHelper.processTouchEvent(ev);
        return true;
    }
5、 关于Callback:
    public static abstract class Callback {
        public void onViewDragStateChanged(int state) {}
            //当ViewDragHelper状态发生变化时回调（STATE_IDLE,STATE_DRAGGING,STATE_SETTLING）
        public void onViewPositionChanged(View changedView, int left, int top, int dx, int dy) {}
            //被拖拽的View位置变化时回调，changedView为位置变化的view，left、top变化后的x、y坐标，dx、dy为新位置与旧位置的偏移量
        public void onViewCaptured(View capturedChild, int activePointerId) {}
            //成功捕获到子View时或者手动调用captureChildView()时回调
        public void onViewReleased(View releasedChild, float xvel, float yvel) {}
            //当前拖拽的view松手或者ACTION_CANCEL时调用，xvel、yvel为离开屏幕时的速率
        public void onEdgeTouched(int edgeFlags, int pointerId) {}
            //当触摸到边界时回调
        public void onEdgeDragStarted(int edgeFlags, int pointerId) {}
            //ACTION_MOVE且没有锁定边缘时触发，在此可手动调用captureChildView()触发从边缘拖动子View
        public boolean onEdgeLock(int edgeFlags)
            //true的时候会锁住当前的边界，false则unLock。锁定后的边缘就不会回调onEdgeDragStarted()
        public int getOrderedChildIndex(int index) {
            //寻找当前触摸点View时回调此方法，如需改变遍历子view顺序可重写此方法
            return index;
        }
        public int getViewHorizontalDragRange(View child) {
            //返回拖拽子View在相应方向上可以被拖动的最远距离，默认为0
            return 0;
        }
        public int getViewVerticalDragRange(View child) {
            //返回拖拽子View在相应方向上可以被拖动的最远距离，默认为0
            return 0;
        }
        public abstract boolean tryCaptureView(View child, int pointerId);
            //对触摸view判断，如果需要当前触摸的子View进行拖拽移动就返回true，否则返回false
        public int clampViewPositionHorizontal(View child, int left, int dx) {
            //拖拽的子View在所属方向上移动的位置，child为拖拽的子View，left为子view应该到达的x坐标，dx为挪动差值
            return 0;
        }
        public int clampViewPositionVertical(View child, int top, int dy) {
            //同上，top为子view应该到达的y坐标
            return 0;
        }
    }
6、关于ViewDragHelper:
    public static ViewDragHelper create(ViewGroup forParent, float sensitivity, Callback cb)
        //sensitivity越大,对滑动的检测就越敏感,默认传1即可，  用来设置滑动的最小检测距离
    public void setEdgeTrackingEnabled(int edgeFlags)
        //设置允许父View的某个边缘可以用来响应托拽事件，
    public boolean shouldInterceptTouchEvent(MotionEvent ev)
        //在父view onInterceptTouchEvent方法中调用
    public void processTouchEvent(MotionEvent ev)
        //在父view onTouchEvent方法中调用
    public void captureChildView(View childView, int activePointerId)
        //在父View内捕获指定的子view用于拖曳，会回调tryCaptureView()
    public boolean smoothSlideViewTo(View child, int finalLeft, int finalTop)
        //某个View自动滚动到指定的位置，初速度为0，可在任何地方调用，动画移动会回调continueSettling(boolean)方法，直到结束
    public boolean settleCapturedViewAt(int finalLeft, int finalTop)
        //以松手前的滑动速度为初值，让捕获到的子View自动滚动到指定位置，只能在Callback的onViewReleased()中使用，其余同上
    public void flingCapturedView(int minLeft, int minTop, int maxLeft, int maxTop)
        //以松手前的滑动速度为初值，让捕获到的子View在指定范围内fling惯性运动，只能在Callback的onViewReleased()中使用，其余同上
    public boolean continueSettling(boolean deferCallbacks)
        //在调用settleCapturedViewAt()、flingCapturedView()和smoothSlideViewTo()时，该方法返回true，一般重写父view的computeScroll方法，进行该方法判断
    public void abort()
        //中断动画

    在ViewDragHelper的滑动中共有三个方法可以调用，smoothSlideViewTo、settleCapturedViewAt、flingCapturedView，
    动画移动会回调continueSettling(boolean)方法，在内部是用的ScrollerCompat来实现滑动的。
    在computeScroll方法中判断continueSettling(boolean)的返回值，来动态刷新界面：
     @Override
     public void computeScroll() {
          if (mViewDragHelper.continueSettling(true)) {
              invalidate();
          }
     }

7、为什么setOnClickable之后，就不能拦截了。
    如果是clickable的话，子view就消费事件了。
    鸿洋：
    原因是什么呢？主要是因为，如果子View不消耗事件，那么整个手势（DOWN-MOVE*-UP）都是直接进入onTouchEvent，
    在onTouchEvent的DOWN的时候就确定了captureView。如果消耗事件，那么就会先走onInterceptTouchEvent方法，判断是否可以捕获，
    而在判断的过程中会去判断另外两个回调的方法：getViewHorizontalDragRange和getViewVerticalDragRange，只有这两个方法返回大于0的值才能正常的捕获。
    这个解释有个问题：应该是先拦截，在判断是否消费。
                     怎么是先判断是否消费，再决定是否拦截呢。

8、 a.STATE_IDLE：所有的View处于静止空闲状态
    b.STATE_DRAGGING：某个View正在被用户拖动（用户正在与设备交互）
    c.STATE_SETTLING：某个View正在安置状态中（用户并没有交互操作），就是自动滚动的过程中

9、down时，子view的 state != STATE_DRAGGING,onInterceptTouchEvent 返回false
   进入 group 的onTouchEvent，也就是DragHelper的 processTouchEvent(event)
   只有 processTouchEvent(event)返回true时，否则onTouchEvent()方法无法接收接下来的ACTION_MOVE等事件）
   同时之后的 ACTION_MOVE、ACTION_UP等事件再来时 就不会进入 onInterceptTouchEvent



Android应用ViewDragHelper详解及部分源码浅析 - 工匠若水 - CSDN博客
http://blog.csdn.net/yanbober/article/details/50419059

强大的ViewDragHelper和ViewDragHelper的妙用 一 - 大叔的愤怒，你驾驭不了 - CSDN博客
http://blog.csdn.net/jaysong2012/article/details/46912875
自定义控件辅助神器ViewDragHelper - 简书
http://www.jianshu.com/p/e4d1f88ca922
Viewdraghelper解析 · Mxn
http://souly.cn/%E6%8A%80%E6%9C%AF%E5%8D%9A%E6%96%87/2015/09/23/viewDragHelper%E8%A7%A3%E6%9E%90/
Android ViewDragHelper完全解析 自定义ViewGroup神器 - Hongyang - CSDN博客
http://blog.csdn.net/lmj623565791/article/details/46858663



ViewDragHelper的简单分析（一) - 菜鸟博客 - CSDN博客
http://blog.csdn.net/chunqiuwei/article/details/50778842
Android ViewDragHelper源码解析 - 随心而悦StayReal - 博客园
http://www.cnblogs.com/lqstayreal/p/4500219.html
ViewDragHelper详解 - pi9nc的专栏 - CSDN博客
http://blog.csdn.net/pi9nc/article/details/39583377
ViewDragHelper详解 - 泡在网上的日子
http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/0911/1680.html


