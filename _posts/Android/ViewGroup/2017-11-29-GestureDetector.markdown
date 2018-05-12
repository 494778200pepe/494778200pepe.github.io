---
layout: post
title:  "GestureDetector"
date:   2017-11-30 16:45:00 +0800
categories: Android
tags: 自定义ViewGroup
author: pepe
description: 『 GestureDetector 』
---
### GestureDetector

GestureDetector（Gesture：手势Detector：识别）

### 构造方法
~~~
GestureDetector gestureDetector=new GestureDetector(GestureDetector.OnGestureListener listener);  
GestureDetector gestureDetector=new GestureDetector(Context context,GestureDetector.OnGestureListener listener);  
GestureDetector gestureDetector=new GestureDetector(Context context,GestureDetector.SimpleOnGestureListener listener);
~~~

### 回调接口
~~~
public interface OnGestureListener {
                // Touch down时触发, e为down时的MotionEvent
                boolean onDown(MotionEvent e);
                // 在Touch down之后一定时间（115ms）触发，e为down时的MotionEvent
                void onShowPress(MotionEvent e);
                // Touch up时触发，e为up时的MotionEvent
                boolean onSingleTapUp(MotionEvent e);
                // 滑动时触发，e1为down时的MotionEvent，e2为move时的MotionEvent
                boolean onScroll(MotionEvent e1, MotionEvent e2, float distanceX, float distanceY);
                // 在Touch down之后一定时间（500ms）触发，e为down时的MotionEvent
                void onLongPress(MotionEvent e);
                // 滑动一段距离，up时触发，e1为down时的MotionEvent，e2为up时的MotionEvent
                boolean onFling(MotionEvent e1, MotionEvent e2, float velocityX, float velocityY);
}
 
public interface OnDoubleTapListener {
                // 完成一次单击，并确定没有二击事件后触发（300ms），e为down时的MotionEvent
                boolean onSingleTapConfirmed(MotionEvent e);
                // 第二次单击down时触发，e为第一次down时的MotionEvent
                boolean onDoubleTap(MotionEvent e);
                // 第二次单击down,move和up时都触发，e为不同时机下的MotionEvent
                boolean onDoubleTapEvent(MotionEvent e);
}
~~~

参考：

[浅析GestureDetector - xirihanlin - 博客园](https://www.cnblogs.com/xirihanlin/archive/2010/12/29/1920356.html)

[Android_GestureDetector手势滑动使用 - CSDN博客](http://blog.csdn.net/y22222ly/article/details/51462674)

