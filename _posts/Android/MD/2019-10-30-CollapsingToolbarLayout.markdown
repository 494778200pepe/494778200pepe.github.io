---
layout: post
title:  "CollapsingToolbarLayout"
date:   2019-10-30 17:06:00 +0800
categories: Android
tags: MD
author: pepe
description: 『 CollapsingToolbarLayout 』
---

CollapsingToolbarLayout继承自FrameLayout，作用是为Toolbar提供了折叠功能。
下面介绍一些参数：

* app:contentScrim这个参数可以让CollapsingToolbarLayout在收缩的时候，背景图片消失的时候，指定一个颜色。

* app:collapsedTitleGravity 指定折叠状态的标题如何放置，可选值:top、bottom等

* app:collapsedTitleTextAppearance指定折叠状态标题文字的样貌

* app:expandedTitleTextAppearance指定展开状态标题文字的样貌

* app:expandedTitleGravity  展开状态的标题如何放置

* app:titleEnabled指定是否显示标题文本app:toolbarId指定与之关联的ToolBar，如果未指定则默认使用第一个被发现的ToolBar子View

* app:expandedTitleMarginStart指定展开状态标题距离开始位置的高度

* app:expandedTitleMarginBottom指定展开状态标题距离底部的距离

* app:expandedTitleMarginEnd指定展开状态标题距离结束位置的高度

* app:layout_collapseParallaxMultiplier="0.7"设置视差的系数，介于0.0-1.0之间。

* app:layout_collapseMode“pin”：固定模式，在折叠的时候最后固定在顶端；“parallax”：视差模式，在折叠的时候会有个视差折叠的效果。

* app:layout_anchor``app:layout_anchorGravity两个属性连同一起，与某一个AppBarLayout控件相关联，确定位置。



### **layout_collapseMode**
CollapsingToolbarLayout是一个FrameLayout，它内部能有多个子元素，而子元素也会有不同的表现。
比如说，在上面的GIF图中，toolbar在缩放后是固定在顶部的，而imageview则是随着布局的滚动而滚动，也即存在一个相对滚动的过程。
所以这些子元素可以添加layout_collapseMode标志位进而产生不同的行为。其实这里也只有两种标志位，分别是：

* pin：有该标志位的View在页面滚动的过程中会一直停留在顶部，比如Toolbar可以被固定在顶部

* parallax：有该标志位的View表示能和页面同时滚动。

* 与该标志位相关联的一个属性是：layout_collapseParallaxMultiplier，该属性是视差因子，表示该View与页面的滚动速度存在差值，造成一种相对滚动的效果。

























参考：

CollapsingToolbarLayout源码分析 - 简书
https://www.jianshu.com/p/8ee6e8a35071


















