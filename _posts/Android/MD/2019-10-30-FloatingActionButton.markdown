---
layout: post
title:  "FloatingActionButton"
date:   2019-10-30 17:25:00 +0800
categories: Android
tags: MD
author: pepe
description: 『 FloatingActionButton 』
---

### **FloatingActionButton**
> FloatingActionButton从本质讲就是一个ImageView，从FloatingActionButton的继承来看，它首先继承了ImageButton，然后是ImageButton继承了ImageView。
所以FloatingActionButton是重写ImageView的，所有FloatingActionButton拥有ImageView的一切属性。FloatingActionButton顾名思义就是一个浮动按钮。

### **FloatingActionButton属性介绍**

由于FloatingActionButton本质上是ImageView，跟ImageView相关的就不介绍，这里重点介绍新加的几个属性。

* app:fabSize="normal"：FAB 的大小，有两种赋值分别是 “mini” 和 “normal”，对应的大小分别为 56dp 和 40dp,默认是“normal”.

* app:elevation="6dp"：阴影的高度，elevation是Android 5.0中引入的新属性，设置该属性使控件有一个阴影，感觉该控件像是“浮”起来一样，这样达到3D效果。对应的方法：setCompatElevation(float)

* app:backgroundTint="#31bfcf"：设置 FAB 的背景颜色，不设置，默认使用theme中colorAccent的颜色。这里需要注意的是，backgroundTint这个属性的值是一个 ColorStateList 类型，如果你误用一个 colors 文件中定义的一个颜色值，在点击时将无法产生涟漪（ripple）交互效果。

* android:clickable="true"：当你使用了backgroundTint属性来改变系统的背景红色，一定要记得设置clickable属性为true，否则也是无法产生涟漪（ripple）交互效果的

* app:rippleColor="#e7d16b"：点击FAB时，形成的波纹颜色。其默认值为 theme 中的colorControlHighlight

* app:borderWidth: 该属性如果不设置0dp，那么在4.1的sdk上FAB会显示为正方形，而且在5.0以后的sdk没有阴影效果。所以设置为borderWidth=“0dp”

* app:pressedTranslationZ：FAB 点击时阴影的大小

* app:useCompatPadding： 是否使用兼容的填充大小

在 Android 5.x 中 FAB 存在的一些问题：

* 没有阴影：

	。解决方法：设置app:borderWidth="0dp"

* 按上述设置后，阴影出现了，但是竟然有矩形的边界！

	。解决方法：需要设置一个 margin 的值。在5.0之前，会默认就有一个外边距（不过并非是margin，只是效果相同）。

综上，FAB 健壮的布局为：增加如下两行配置
```
app:borderWidth="0dp"
android:layout_margin="20dp"
```











参考：

[Material Design系列教程（2） - FloatingActionButton - 简书](https://www.jianshu.com/p/f2f210ad4f70)

[Android Material Design系列之FloatingActionButton和Snackbar](https://mp.weixin.qq.com/s?__biz=MjM5NDkxMTgyNw==&mid=2653057631&idx=1&sn=445e98c146a44c06bef683cd5f56f454&scene=21#wechat_redirect)

[FloatingActionButton属性、用法，以及解析并解决sdk25以上只隐藏不显示的问题 - GSQ_Cat - CSDN博客](https://blog.csdn.net/chen_xi_hao/article/details/74347023)

Material Design之-交互效果炸裂的 FloatingActionMenu - 掘金
https://juejin.im/post/5c87c1efe51d453b7666c26b













