---
layout: post
title:  "MotionEvent"
date:   2018-05-26 14:27:10 +0800
categories: Android
tags: 自定义View
author: pepe
description: 『 MotionEvent 』
---

常规:
    ACTION_DOWN: 表示用户开始触摸.
    ACTION_MOVE: 表示用户在移动(手指或者其他)
    ACTION_UP:表示用户抬起了手指
    ACTION_CANCEL:表示手势被取消了,一些关于这个事件类型的讨论见:http://stackoverflow.com/questions/11960861/what-causes-a-motionevent-action-cancel-in-android
不常见:
    ACTION_OUTSIDE: 表示用户触碰超出了正常的UI边界.
多点触控：
    但是对于多点触控的支持,Android加入了以下一些事件类型.来处理,如另外有手指按下了,有的手指抬起来了.等等:
    ACTION_POINTER_DOWN:有一个非主要的手指按下了.
    ACTION_POINTER_UP:一个非主要的手指抬起来了

    getEdgeFlags():当事件类型是ActionDown时可以通过此方法获得,手指触控开始的边界. 如果是的话,有如下几种值:EDGE_LEFT,EDGE_TOP,EDGE_RIGHT,EDGE_BOTTOM


Action的值在0x0000到0xffff之间
    低八位表示 action 的类型，如 down、move、down
    高八位表示 是第几个触控点
    基本变量：
    public static final int ACTION_MASK             = 0xff;
    public static final int ACTION_POINTER_INDEX_MASK  = 0xff00;
    public static final int ACTION_POINTER_INDEX_SHIFT = 8;

    获取action：action & ACTION_MASK ，得到低八位值
    获取触控点：(mAction & ACTION_POINTER_INDEX_MASK) >> ACTION_POINTER_INDEX_SHIFT，获取高八位值，然后移位

    计算机始终还是以位来存储信息的。如果我们多我熟悉以位为基本单位来理解信息的存储。对于理解android中的很多变量是很有帮助的。
    因为他其中的很多东西使用的这样的节约内在的技巧。
    如onMeasure中的MeasureSpec。


参考blog：
    android触控,先了解MotionEvent(一) - 李海珍的个人页面
    https://my.oschina.net/banxi/blog/56421


