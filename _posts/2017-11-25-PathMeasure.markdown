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

构造方法
==========

|方法名|释义|
|-|-|
|PathMeasure()|创建一个空的PathMeasure|
|PathMeasure(Path path, boolean forceClosed)|创建 PathMeasure 并关联一个指定的Path(Path需要已经创建完成)|

公共方法
==========

|方法体	|释义|
|-|-|
|void       setPath(Path path, boolean forceClosed)										|关联一个Path|
|boolean    isClosed()																	|是否闭合|
|float      getLength()																	|获取Path的长度|
|boolean    nextContour()																|跳转到下一个轮廓|
|boolean    getSegment(float startD, float stopD, Path dst, boolean startWithMoveTo)	|截取片段|
|boolean    getPosTan(float distance, float[] pos, float[] tan)							|获取指定长度的位置坐标及该点切线值|
|boolean    getMatrix(float distance, Matrix matrix, int flags)							|获取指定长度的位置坐标及该点Matrix|

代码
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

