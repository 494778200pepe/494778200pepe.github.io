---
layout: post
title:  "TabLayout"
date:   2019-10-30 17:13:00 +0800
categories: Android
tags: MD
author: pepe
description: 『 TabLayout 』
---


### **TabLayout 的常用属性**

* app:tabMode	设置标签（tab）布局模式。有如下两种选项：

	"fixed"：默认值，表示标签页不能滚动。一般用于标签较少的情况，每个 Tab 可以平分屏幕宽度。

	"scrollable"：表示标签页可以滚动展示。使用于标签文本过长和过多标签页这些情况。该模式通常在结合 ViewPager 中使用。当使用该属性时，tabGravity的设置就不起作用了，标签都是从左向右布局。

	对应代码方法：setTabMode(int)
	
* app:tabGravity	设置标签页（tab）对齐方式。有如下两种选项：

	"fill"：所有标签页（tab）填满 TabLayout 宽度，该选项只有在app:tabMode="fixed"的时候才生效 。

	"center"：标签页居中显示。

	对应代码方法：setTabGravity(int)
	
* app:tabTextColor	未选中标签页时，字体的颜色

* app:tabSelectedTextColor	选中标签页时，字体的颜色

* app:tabIndicatorColor	滑动时，指示器的颜色

* app:tabIndicatorHeight	设置指示器高度

* app:tabBackground	标签页（tab）布局背景

* app:tabTextAppearance	设置标签页标题文字样式

* app:tabPadding	标签页内边距

* app:tabContentStart	标签页（tab）左边距内容偏移量，该选项只有在app:tabMode="scrollable"时才生效

* app:tabMaxWidth	标签页最大宽度

* app:tabMinWidth=	标签页最小宽度

### **TabLayout 的常用方法**

* newTab()	创建一个新的标签

* addTab(Tab)	添加标签页

* removeTab(Tab)	删除标签页

* removeTabAt(int)	通过索引，删除标签页

* removeAllTabs()	删除所有标签页

* getTabCount()	返回标签页数量

* getTabAt(int)	返回索引对应的标签 页

* getSelectedTabPosition()	返回当前选中的标签页索引，若未选中，则返回 -1

* addOnTabSelectedListener(OnTabSelectedListener)	添加监听器

* removeOnTabSelectedListener(OnTabSelectedListener)	移除监听器

* clearOnTabSelectedListeners()	清除所有的监听器

* addView	TabLayout 是一个ViewGroup，所以可以直接添加子View

* setupWithViewPager(ViewPager)	TabLayout 和 ViewPager 关联，随着 ViewPager 滑动而切换标签。

	注：TabLayout 与 ViewPager 关联后，TabLayout 的标签页（tab）数量由 ViewPager 分页数量决定，
	TabLayout 的标签内容由 ViewPager的Adapter中getPagerTitle()方法返回的内容决定



参考：

[Material Design系列教程（8） - TabLayout - 简书](https://www.jianshu.com/p/2042f5bf9122)










