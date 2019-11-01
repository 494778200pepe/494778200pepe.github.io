---
layout: post
title:  "CollapsingToolbarLayout"
date:   2019-10-30 17:06:00 +0800
categories: Android
tags: MD
author: pepe
description: 『 CollapsingToolbarLayout 』
---

`CollapsingToolbarLayout` 通常用来在布局中包裹一个 `Toolbar`，是对 `Toolbar` 的再次封装，以实现具有“折叠效果“的顶部栏。
`CollapsingToolbarLayout` 是不能独立存在的，它必须只能作为 `AppBarLayout` 的直接子 View 来使用。

`CollapsingToolbarLayout` 具备以下几个特点：

* 可折叠标题（Collapsing title）

	当布局完全显示时，标题最大化；当布局逐渐移出屏幕时，标题逐渐变小。
	可以通过 setTitle(CharSequence) 来设置显示的标题。
	标题外观可以通过`collapsedTextAppearance`和`expandedTextAppearance`进行调整。

* 内容纱布（Content scrim）

	当滚动位置到达临界点时，内容纱布会显示或隐藏。
	可以通过 `setContentScrim(Drawable)` 来设置内容纱布。

* 状态栏纱布（Status bar scrim)

	当滚动位置到达临界点时，状态栏纱布会显示或隐藏（位于状态栏后）。
	可以通过 `setStatusBarScrim(Drawable)` 来设置状态栏纱布。
	状态栏纱布只能支持 `Android 5.0 LOLLIPOP` 及以上的设备，并且还要设置`android:fitsSystemWindows="true"`。

* 视差滚动子 View（Parallax scrolling children）

	子 View 可以选择以“视差”的方式来进行滚动。
	（视觉效果上就是子 View 滚动的比其他 View 稍微慢些）。
	设置方法为：`app:layout_collapseMode="parallax"`

* 子 View 位置固定（Pinned position children）

	子 View 可以选择是否在全局空间上固定位置，这对于 Toolbar 来说非常有用，
	因为当布局在移动时，可以将 Toolbar 固定位置而不受移动的影响。 
	设置方法为：`app:layout_collapseMode="pin"`。

注：不要在运行时手动为 `Toolbar` 添加子View。在运行期间，`Toolbar` 会自动添加一个“虚拟的 View” 从而让我们能为 标题 计算出可用空间，
如果你手动添加子View，这个过程会被干扰。



`CollapsingToolbarLayout`继承自`FrameLayout`，作用是为`Toolbar`提供了折叠功能。
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

[玩转AppBarLayout，更酷炫的顶部栏 - huachao1001的专栏 - CSDN博客](https://blog.csdn.net/huachao1001/article/details/51558835)

[CollapsingToolbarLayout源码分析 - 简书](https://www.jianshu.com/p/8ee6e8a35071)


















