---
layout: post
title:  "PathMeasure"
date:   2017-11-25 10:58:00 +0800
categories: Android
tags: 绘制
author: pepe
description: 『 PathMeasure 』
---

Path & PathMeasure
============
顾名思义，PathMeasure是一个用来测量Path的类。
# 介绍
## 构造方法

~~~
PathMeasure()
~~~
* 创建一个空的PathMeasure。
* 用这个构造函数可创建一个空的用这个构造函数可创建一个空的 PathMeasure，但是使用之前需要先调用 setPath 方法来与 Path 进行关联。
        被关联的 Path 必须是已经创建好的，如果关联之后 Path 内容进行了更改，则需要使用 setPath 方法重新关联。
		
~~~
PathMeasure(Path path, boolean forceClosed)
~~~
* 创建 PathMeasure 并关联一个指定的Path(Path需要已经创建完成)。
* 第一个参数自然就是被关联的 Path 了，第二个参数是用来确保 Path 闭合，如果设置为 true，
			则不论之前Path是否闭合，都会自动闭合该 Path(如果Path可以闭合的话)。
* 不论 forceClosed 设置为何种状态(true 或者 false)， 都不会影响原有Path的状态，即 Path 与 PathMeasure 关联之后，之前的的 Path 不会有任何改变。
* forceClosed 的设置状态可能会影响测量结果，如果 Path 未闭合但在与 PathMeasure 关联的时候设置 forceClosed 为 true 时，
            测量结果可能会比 Path 实际长度稍长一点，获取到到是该 Path 闭合时的状态。

## 公共方法

~~~
void       setPath(Path path, boolean forceClosed)	
~~~									
关联一个Path
~~~
boolean    isClosed()	
~~~																
是否闭合
~~~
float      getLength()
~~~		
获取Path的长度
~~~														
boolean    nextContour()
~~~																
跳转到下一个轮廓
~~~
boolean    getSegment(float startD, float stopD, Path dst, boolean startWithMoveTo)	
~~~		
截取片段
~~~		
boolean    getPosTan(float distance, float[] pos, float[] tan)
~~~
获取指定长度的位置坐标及该点切线值
~~~
boolean    getMatrix(float distance, Matrix matrix, int flags)
~~~							
获取指定长度的位置坐标及该点Matrix

# 使用
==========
~~~
	//测试圆和矩形的起始点，中心均为(0,0)
        //圆的起始点（radius，0）
        Path circlePath = new Path();
        circlePath.addCircle(0,0,250, Path.Direction.CW);
        PathMeasure measure3 = new PathMeasure(circlePath,false);
        Path circleTestPath = new Path();
        measure3.getSegment(0,300,circleTestPath,true);
        mPaint.setColor(Color.GREEN);
        canvas.drawPath(circleTestPath,mPaint);

        //矩形的起始点（-radius，-radius）
        Path rectPah = new Path();
        rectPah.addRect(-300,-300,300,300, Path.Direction.CW);
        PathMeasure measure4 = new PathMeasure(rectPah,false);
        Path rectTestPath = new Path();
        measure4.getSegment(0,400,rectTestPath,true);
        canvas.drawPath(rectTestPath,mPaint);
~~~


表一
==========

|---
| Default aligned | Left aligned | Center aligned | Right aligned
|-|:-|:-:|-:
| First body part | Second cell | Third cell | fourth cell
| Second line |foo | **strong** | baz
| Third line |quux | baz | bar
|---
| Second body
| 2 line
|===
| Footer row

表二
==========

|-----------------+------------+-----------------+----------------|
| Default aligned |Left aligned| Center aligned  | Right aligned  |
|-----------------|:-----------|:---------------:|---------------:|
| First body part |Second cell | Third cell      | fourth cell    |
| Second line     |foo         | **strong**      | baz            |
| Third line      |quux        | baz             | bar            |
|-----------------+------------+-----------------+----------------|
| Second body     |            |                 |                |
| 2 line          |            |                 |                |
|=================+============+=================+================|
| Footer row      |            |                 |                |
|-----------------+------------+-----------------+----------------|

表三
==========
| First cell|Second cell|Third cell
| First | Second | Third |

First | Second | | Fourth |

