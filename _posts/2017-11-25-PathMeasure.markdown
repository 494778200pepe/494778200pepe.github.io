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






