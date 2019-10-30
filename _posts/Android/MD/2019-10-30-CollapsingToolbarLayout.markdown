---
layout: post
title:  "CollapsingToolbarLayout"
date:   2019-10-30 17:06:00 +0800
categories: Android
tags: MD
author: pepe
description: 『 CollapsingToolbarLayout 』
---


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


















