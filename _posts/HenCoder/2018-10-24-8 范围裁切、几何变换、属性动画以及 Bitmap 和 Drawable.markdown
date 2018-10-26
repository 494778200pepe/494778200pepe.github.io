---
layout: post
title:  "8 范围裁切、几何变换、属性动画以及 Bitmap 和 Drawable"
date:   2018-10-24 21:15:00 +0800
categories: HenCoder
tags: HenCoder_plus
author: pepe
description: 『 范围裁切、几何变换、属性动画以及 Bitmap 和 Drawable 』
---

### **文字测量**

#### 完全包裹文字
```
Rect rect  = new Rect();
paint.getTextBounds("abab",0,"abab".length(),rect);
float offset = (rect.ascent + rect.descent)/2;
canvas.drawText("abab",getWidth()/2,getHeight()/2 - offset,paint);
```


#### 包含文字上下空白，避免文字变化时抖动
```
Paint.FontMetrics fontMetrics = new Paint.FontMetrics();
paint.getFontMetrics(fontMetrics);
float offset = (fontMetrics.ascent + fontMetrics.descent)/2;
canvas.drawText("abab",getWidth()/2,getHeight()/2 - offset,paint);
```

### **设置字体**
```
paint.setTypeface(Typeface.createFromAsset(getContext().getAssets(),"Quicksand-Regual.ttf"));
```


### **去除文字左边空白**
```
Rect rect  = new Rect();
paint.getTextBounds("abab",0,"abab".length(),rect);
canvas.drawText("abab", -rect.left, 200, paint);
```






