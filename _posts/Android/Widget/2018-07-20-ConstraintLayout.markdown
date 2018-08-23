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


### **bias**
数值范围[0, 1]
```
layout_constraintHorizontal_bias

layout_constraintVertical_bias
```

### **大小**
```
app:layout_constraintWidth_default="XXX"，XXX取值为spread或wrap，默认值为spread，占用约束内所有的空间；如果取值为wrap，并且该view的layout_width="0dp"或者layout_width="wrap_content"，view的效果和layout_width="wrap_content"效果一样，同时view不会超出约束限制。

app:layout_constraintWidth_max="XXX"，XXX是dp单位数值，表示在有约束且layout_width="0dp"的情况下控件显示的最大宽度，现在可以控制最大显示宽度

app:layout_constraintWidth_min="XXX"，XXX是dp单位数值，表示最小的显示宽度。

app:layout_constraintHeight_max="XXX", 同理。

app:layout_constraintHeight_min="XXX"，同理。

```

### **其他**
```
app:layout_constraintBaseline_creator=""

app:layout_constraintTop_creator=""

app:layout_constraintBottom_creator=""

app:layout_constraintLeft_creator=""

// 右侧 约束空间gone时，距离右侧的值
layout_goneMarginRight = ""
layout_goneMarginLeft = ""
layout_goneMarginTop = ""
layout_goneMarginBottom = ""
```


[Android新特性介绍，ConstraintLayout完全解析 - CSDN博客](https://blog.csdn.net/guolin_blog/article/details/53122387)

[解析ConstraintLayout的性能优势](https://mp.weixin.qq.com/s?__biz=MzAwODY4OTk2Mg==&mid=2652044589&idx=1&sn=36f09ada2b279b0c56fcd91085ebe93a&chksm=808d5d68b7fad47e4de2704b24e51fd57799d19f1f7b334aaa9bfa2671c34ca8cc6bcd493882&scene=21#wechat_redirect)

[拒绝拖拽 使用ConstraintLayout优化你的布局吧](https://mp.weixin.qq.com/s/vI-fPaNoJ7ZBlZcMkEGdLQ)

[Android高性能布局之ConstraintLayout](https://mp.weixin.qq.com/s/aVkX88v-SUiFoh8UKPRTgQ)

[代码实验室--带你一步步理解使用 ConstraintLayout - 简书](https://www.jianshu.com/p/793f76cf9fea)

[ConstraintLayout 完全解析 快来优化你的布局吧 - CSDN博客](https://blog.csdn.net/lmj623565791/article/details/78011599?utm_source=tuicool&utm_medium=referral)

[ConstraintLayout 用法全解析 - 简书](https://www.jianshu.com/p/502127a493fb)



















