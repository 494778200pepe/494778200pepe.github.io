---
layout: post
title:  "LayoutParams"
date:   2017-11-27 20:14:00 +0800
categories: Android
tags: 自定义ViewGroup
author: pepe
description: 自定义ViewGroup.
---

    1、 ViewGroup中有两个内部类ViewGroup.LayoutParams和ViewGroup.MarginLayoutParams，MarginLayoutParams继承自LayoutParams。   
    2、 这两个内部类就是ViewGroup的布局参数类。
    3、 比如我们在LinearLayout等布局中使用的layout_width\layout_hight等以“layout_”开头的属性都是布局属性。 
    4、 在View中有一个mLayoutParams的变量用来保存这个View的所有布局属性。
    5、 ViewGroup.LayoutParams有两个属性layout_width和layout_height，因为所有的容器都需要设置子控件的宽高，
    6、 所以这个LayoutParams是所有布局参数的基类，如果需要扩展其他属性，都应该继承自它。
    7、 比如RelativeLayout中就提供了它自己的布局参数类RelativeLayout.LayoutParams，并扩展了很多布局参数，
    8、 我们平时在RelativeLayout中使用的布局属性都来自它。
    
View在 Layout 时，必须要知道父ViewGroup 的 LayoutParams。从哪儿来呢？

generateLayoutParams()，这个方法主要是用于父容器添加子View时调用。

在ViewGroup中有下面几个关于LayoutParams的方法：
~~~
/**
 * 根据xml文件中的宽和高得生成相应的LayoutParams
 * Layout_width 和 Layout_height 都是ViewGroup的内部类LayoutParams的成员变量
*/
@Override
public LayoutParams generateLayoutParams(AttributeSet attrs) {
    return new CustomLayoutParams(getContext(), attrs);
}

@Override
protected ViewGroup.LayoutParams generateLayoutParams(ViewGroup.LayoutParams p) {
    return new CustomLayoutParams (p);
}

/**
 * 如果ViewGroup的generateLayoutParams()返回值为空，那么会启用此方法
*/
@Override
protected LayoutParams generateDefaultLayoutParams() {
    return new CustomLayoutParams (LayoutParams.MATCH_PARENT , LayoutParams.MATCH_PARENT);
}


/**
 * checkLayoutParams 这个方法在更新或者设置LayoutParams的时候会调用
 * 如果不重写，可能会导致设置的属性无效
*/
@Override
protected boolean checkLayoutParams(ViewGroup.LayoutParams p) {
    return p instanceof CustomLayoutParams ;
}
~~~

   
   

#### 参考：

[Android自定义ViewGroup（四、打造自己的布局容器） - CSDN博客](http://blog.csdn.net/xmxkf/article/details/51500304#￢ﾑﾢ-￩ﾇﾍ￥ﾆﾙgeneratelayoutparams)

[LayoutParams，setContentView，generateDefaultLayoutParams - pageTan的小基地 - CSDN博客](http://blog.csdn.net/u013818990/article/details/50570944)

[自定义LayoutParams - pageTan的小基地 - CSDN博客](http://blog.csdn.net/u013818990/article/details/50603889)



