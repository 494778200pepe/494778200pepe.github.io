---
layout: post
title:  "ScrollView嵌套RecyclerView出现item显示不全"
date:   2019-07-08 15:32:00 +0800
categories: Android
tags: BUG
author: pepe
description: 『 ScrollView嵌套RecyclerView出现item显示不全 』
---

出现问题不要慌，耐心解决才是王道，哈哈。首先说下出现这个问题的情景吧，首先声明这个问题在23版本以上出现的，23版本是android 6.0版本，是的当我们targetSdkVersion=23的时候（也就是我们兼容到23版本）是没有问题的，一但兼容到23版本以上就会出现这个问题，这个坑也是第一次踩到，发表个博客警戒下自己，同时也希望可以帮助你顺利的填上这个坑。

首先这个问题的解决方案有3种（有效的，无效的咱们略过）

### **第一种：**
既然这个问题是23版本以上的问题（不包括23版本），那么我们就只兼容到23版本就OK喽，就是修改app的build.gradle的targetSdkVersion = 23 和 compileSdkVersion = 23，这个问题就可以顺利解决，但是现在市场上7.0可以说已经普遍了，8.0手机也已经出了，只兼容到6.0版本显然不是我们想要的结果，那么接下来还有2种解决方案，搬个板凳坐下来认真听下面两种吧。

### **第二种：**

在你的`RecyclerView`上再嵌套一层`RelativeLayout`然后添加属性 `android:descendantFocusability="blocksDescendants"`，既然提到了这个属性就说下它的意思吧，知道的同学再复习一遍呗，巩固巩固更牢靠。

首先该属性`android:descendantFocusability`的含义是：当一个`view`获取焦点时，定义`ViewGroup`和其子控件两者之间的关系。

它一共有3个属性值，它们分别是：

`beforeDescendants`：viewGroup会优先子类控件而获取焦点

`afterDescendants`：viewGroup只有当子类控件不需要获取焦点的时候才去获取焦点

`blocksDescendants`：viewGroup会覆盖子类控件而直接获取焦点

想必了解了这个属性之后，你就会恍然大悟啦。
```
<RelativeLayout
     android:layout_width="match_parent"
     android:layout_height="wrap_content"
     android:descendantFocusability="blocksDescendants">
        <android.support.v7.widget.RecyclerView
             android:id="@+id/rv_me_window"
             android:layout_width="match_parent"
             android:layout_height="match_parent"
             android:paddingLeft="16dp"
             android:paddingRight="16dp"
             android:overScrollMode="never"/>
</RelativeLayout>
```
### **第三种：**

当然这种方式是今天偶然在网上看到的，听说既可以填item显示不全的坑，又可以填嵌套滑动卡顿的坑。是不是很期待？哈哈，终极解决方案总是要在最后闪亮登场滴，不过这种方式本人没有测试过，不知道具体的可靠性，同学们需要自行测试，本人用的是第二种方案。

方案是这样的：首先在xml布局中将你的`crollView`替换成`android.support.v4.widget.NestedScrollView`，并在java代码中设置`recyclerView.setNestedScrollingEnabled(false);`

这样就可以完美解决啦。`NestedScrollView`这个东西是5.0的新控件，如果不了解的同学可以自行百度，网上对它的解释有很多，也有很多例子，当时我用它的时候，好像是跟做toolbar联动的时候用过，不过已经记得不太深刻了，好久没有使用这个了，印象当中使用这个好像还需要手动依赖`compile 'com.android.support:design:25.0.1'`这个库。

参考：

[解决ScrollView嵌套RecyclerView出现item显示不全的问题 - Sky * ZST - CSDN博客](https://blog.csdn.net/u012246458/article/details/81201408)


