---
layout: post
title:  "PathMeasure"
date:   2017-11-25 10:58:00 +0800
categories: Android
tags: 绘制
author: pepe
description: The read me page of pepe‘s blog.
---

Path & PathMeasure
============
顾名思义，PathMeasure是一个用来测量Path的类，主要有以下方法:

#构造方法


|方法名|释义|
|-|-|
|PathMeasure()|创建一个空的PathMeasure|
|PathMeasure(Path path, boolean forceClosed)|创建 PathMeasure 并关联一个指定的Path(Path需要已经创建完成)|

#公共方法
<table>
 	<tr>  
 		<td>返回值</td><td>方法名</td><td>释义</td>
	</tr>
	<tr>  
		<td>void</td><td>setPath(Path path, boolean forceClosed)</td><td>关联一个Path</td>
	</tr>
	<tr>  
		<td>boolean</td><td>isClosed()</td><td>是否闭合</td>
	</tr>
	<tr>  
		<td>float</td><td>getLength()</td><td>获取Path的长度</td>
	</tr>
	<tr>  
		<td>boolean</td><td>nextContour()</td><td>跳转到下一个轮廓</td>
	</tr>
	<tr>  
		<td>boolean</td><td>getSegment(float startD, float stopD, Path dst, boolean startWithMoveTo)</td><td>截取片段</td>
	</tr>
	<tr>  
		<td>boolean</td><td>getPosTan(float distance, float[] pos, float[] tan)</td><td>获取指定长度的位置坐标及该点切线值</td>
	</tr>
	<tr>  
		<td>boolean</td><td>getMatrix(float distance, Matrix matrix, int flags)</td><td>获取指定长度的位置坐标及该点Matrix</td>
	</tr>
</table>


TypedArray | Android Developers
https://developer.android.google.cn/reference/android/content/res/TypedArray.html

简介：Container for an array of values that were retrieved with obtainStyledAttributes(AttributeSet, int[], int, int) or obtainAttributes(AttributeSet, int[]). Be sure to call recycle() when done with them. The indices used to retrieve values from this structure correspond to the positions of the attributes given to obtainStyledAttributes.

翻译过来就是:

 -  一个用来检索属性值的容器;
 -  使用完之后，必须recycle();
 - index下标用来检索属性，相当于属性在obtainStyledAttributes（获取样式属性）中的position.

再来看方法：

|返回值|方法名|
|-|-|-|
|boolean|getBoolean(int index, boolean defValue)|
|int	|getColor(int index, int defValue)|-|
|float	|getDimension(int index, float defValue)
|Drawable	|getDrawable(int index)
|float	|getFloat(int index, float defValue)
|int	|getInt(int index, int defValue)
|int	|getInteger(int index, int defValue)
|String	|getString(int index)
|CharSequence	|getText(int index)

其他方法：

|返回值|方法名|解释|
|-|-|-|
|ColorStateList	|getColorStateList(int index)||
|int	|getDimensionPixelOffset(int index, int defValue)
|int	|getDimensionPixelSize(int index, int defValue)
|float	|getFraction(int index, int base, int pbase, float defValue)
|int	|getIndex(int at)
|int	|getIndexCount()
|int	|getLayoutDimension(int index, int defValue)
|int	|getLayoutDimension(int index, String name)
|String	|getNonResourceString(int index)
|String	|getPositionDescription()
|int	|getResourceId(int index, int defValue)
|Resources	|getResources()
|CharSequence[]	g|etTextArray(int index)
|boolean	|getValue(int index, TypedValue outValue)
|boolean	|hasValue(int index)
|int	|length()
|TypedValue	|peekValue(int index)
|void	|recycle()








