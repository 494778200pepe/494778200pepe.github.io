---
layout: post
title:  "ConstraintLayout"
date:   2018-07-20 18:56:00 +0800
categories: Android
tags: Widget
author: pepe
description: 『 ConstraintLayout 』
---

### **基本属性**
```
layout_constraintLeft_toLeftOf

layout_constraintLeft_toRightOf

layout_constraintRight_toLeftOf

layout_constraintRight_toRightOf

layout_constraintTop_toTopOf

layout_constraintTop_toBottomOf

layout_constraintBottom_toTopOf

layout_constraintBottom_toBottomOf

layout_constraintStart_toEndOf

layout_constraintStart_toStartOf

layout_constraintEnd_toStartOf

layout_constraintEnd_toEndOf


// 文字基线
layout_constraintBaseline_toBaselineOf

```
ConstraintLayout中0代表：MATCH_CONSTRAINT


### **宽高比**
```
app:layout_constraintDimensionRatio="16:6"

app:layout_constraintDimensionRatio="W,16:6"

app:layout_constraintDimensionRatio="H,16:6"
```

### **Guidelines**
```
android:orientation = "vertical|horizontal". // 根据orientation来决定是横向还是纵向

layout_constraintGuide_begin = 30dp // 距离顶部30dp的地方有个辅助线

layout_constraintGuide_end = 30dp // 距离底部

layout_constraintGuide_percent = 0.8 // 0.8即为距离顶部80%

```

### **chainStyle**
```
layout_constraintHorizontal_chainStyle = "spread|packed|spread_inside".


app:layout_constraintHorizontal_weight
app:layout_constraintVertical_weight

```
#### **spread**
默认

#### **packed**

#### **spread_inside**

![constraintLayout]({{ site.baseurl }}/assets/images/android/Widget/constraintLayout.png)


### **bias**
数值范围[0, 1]
```
layout_constraintHorizontal_bias

layout_constraintVertical_bias
```

### **大小**
```
app:layout_constraintWidth_default="XXX"，默认值为spread。

spread:意思是占用所有的符合约束的空间

wrap:匹配内容大小但不超过约束限制，注意和直接指定宽度为wrap_content的区别就是不超过约束限制

wrap_content:匹配内容大小，同时可以超过约束限制
对于wrap_content会超过约束限制，谷歌又新增了如下属性
app:layout_constrainedWidth=”true|false”
app:layout_constrainedHeight=”true|false”

percent:按照父布局的百分比设置,需要layout_constraintWidth_percent设置百分比例
app:layout_constraintWidth_default="percent"
app:layout_constraintWidth_percent="0.4"



app:layout_constraintWidth_max="XXX"，XXX是dp单位数值，表示在有约束且layout_width="0dp"的情况下控件显示的最大宽度，现在可以控制最大显示宽度

app:layout_constraintWidth_min="XXX"，XXX是dp单位数值，表示最小的显示宽度。

app:layout_constraintHeight_max="XXX", 同理。

app:layout_constraintHeight_min="XXX"，同理。

```

### **圆形布局**
ConstraintLayout还提供了一种比较炫酷的圆形布局，这是以往的布局所做不到的。涉及到的属性也很简单，就下面三个：
```
layout_constraintCircle : 圆心，值是某个view的id
layout_constraintCircleRadius : 半径
layout_constraintCircleAngle ：角度，值是从0-360，0是指整上方
```

### **其他**
```
app:layout_constraintBaseline_creator=""

app:layout_constraintTop_creator=""

app:layout_constraintBottom_creator=""

app:layout_constraintLeft_creator=""

// 右侧 约束空间gone时，距离右侧的值(这个只能设置固定的距离)
layout_goneMarginRight = ""
layout_goneMarginLeft = ""
layout_goneMarginTop = ""
layout_goneMarginBottom = ""

```

> Group是一个可以同时控制多个view 可见性的虚拟View。

```
<android.support.constraint.Group
       ...
        android:visibility="invisible"
        app:constraint_referenced_ids="a,c" />
```


> Placeholder:占位布局。他自己本身不会绘制任何内容，但他可以通过设置app:content="id"，将id View的内容绘制到自己的位置上，而原id的 View就像gone了一样。

> Barrier屏障，一个虚拟View。

```
<android.support.constraint.Barrier
              android:id="@+id/barrier"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              app:barrierDirection="end"//end,left,right,top,bottom
              app:constraint_referenced_ids="text1,text2" />
```

则Barrier始终位于text1,text2两个View最大宽度的右边

```
app:constraint_barrierAllowsGoneWidgets = "false" // Barrier不再关注Gone的View
```




参考：

[ConstraintLayout 完全解析 快来优化你的布局吧 - CSDN博客](https://blog.csdn.net/lmj623565791/article/details/78011599?utm_source=tuicool&utm_medium=referral)

[Android新特性介绍，ConstraintLayout完全解析 - CSDN博客](https://blog.csdn.net/guolin_blog/article/details/53122387)

[ConstraintLayout 用法全解析 - 简书](https://www.jianshu.com/p/502127a493fb)

[解析ConstraintLayout的性能优势](https://mp.weixin.qq.com/s?__biz=MzAwODY4OTk2Mg==&mid=2652044589&idx=1&sn=36f09ada2b279b0c56fcd91085ebe93a&chksm=808d5d68b7fad47e4de2704b24e51fd57799d19f1f7b334aaa9bfa2671c34ca8cc6bcd493882&scene=21#wechat_redirect)

[Android高性能布局之ConstraintLayout](https://mp.weixin.qq.com/s/aVkX88v-SUiFoh8UKPRTgQ)

[代码实验室--带你一步步理解使用 ConstraintLayout - 简书](https://www.jianshu.com/p/793f76cf9fea)

[ConstraintLayout 终极秘籍（上） - 云在千峰](http://blog.chengyunfeng.com/?p=1030)


动态图解&实例 ConstraintLayout Chain - 简书
https://www.jianshu.com/p/beb9f7157209





















