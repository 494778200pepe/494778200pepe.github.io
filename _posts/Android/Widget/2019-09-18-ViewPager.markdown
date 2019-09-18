---
layout: post
title:  "ViewPager"
date:   2019-09-18 09:53:00 +0800
categories: Android
tags: Widget
author: pepe
description: 『 ViewPager 』
---

viewpager有一个setPageTransformer()，主要是用在viewpager在滑动时的回调，第一个参数一般为true。它的参数为PageTransformer变量。

 * ViewPager.PageTransformer 是一个接口，里面的方法只有一个transformPage(View view,float position)。
 * 参数含义分别是：当前的view，以及该view在viewpager中的位置。
 * 不同的view，position的值是不同的。停止滑动后，显示在屏幕中的view的position为0。
 * 往左滑时，该view的position会随着滑动的移动而逐渐降低到-1;
 * 往后滑时，position会随着滑动逐渐增加到1。
 * position的值大于1，或者小于-1，那么该position已经不可见。
 * viewpager会保留当前屏幕，左边及右边一共3个view。
 * 超出1，那就代表该view是右边往右，小于-1就代表该view是左边往左。


Android Study 之 平行空间效果实现 - 静心 Study - CSDN博客
https://blog.csdn.net/u012400885/article/details/78726981

ViewPager.PageTransformer详解——打造千变万化的viewPager - musk6的博客 - CSDN博客
https://blog.csdn.net/musk6/article/details/54614384

[Parallax Animation]实现知乎 Android 客户端启动页视差滚动效果
http://ryanhoo.github.io/blog/2014/07/16/step-by-step-implement-parallax-animation-for-splash-screen-of-zhihu/








