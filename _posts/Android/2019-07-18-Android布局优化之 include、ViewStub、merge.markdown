---
layout: post
title:  "Android布局优化之 include、ViewStub、merge"
date:   2019-07-18 14:24:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 include、ViewStub、merge 』
---

### **1、include**

`include` 就是为了解决重复定义相同布局的问题。

> 注意：如果 `include` 中设置了id，那么就通过 `include` 的id来查找被 `include布局根元素的View`；如果 `include` 中没有设置Id, 而被 `include的布局的根元素` 设置了id，那么通过该 `根元素的id` 来查找该 `view` 即可。拿到根元素后查找其子控件都是一样的。

### **2、ViewStub**

`ViewStub` 就是一个宽高都为0的一个View，它默认是不可见的，只有通过调用 `setVisibility` 函数或者 `Inflate` 函数才会将其要装载的目标布局给加载出来，从而达到延迟加载的效果。

```
<ViewStub  
    android:id="@+id/stub_import"  
    android:inflatedId="@+id/stub_comm_lv"  
    android:layout="@layout/my_comment_layout"  
    android:layout_width="fill_parent"  
    android:layout_height="wrap_content"  
    android:layout_gravity="bottom" /  
    
// my_comment_layout.xml
<?xml version="1.0" encoding="utf-8"?>  
<ListView xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:id="@+id/my_comm_lv"  
    android:layout_height="match_parent" >  

</ListView>  

        // 方式1，获取ViewStub,  
        ViewStub listStub = (ViewStub) findViewById(R.id.stub_import);  
        
        // 加载评论列表布局  
        listStub.setVisibility(View.VISIBLE);  
        
        // 获取到评论ListView，注意这里是通过ViewStub的inflatedId来获取  
        ListView commLv = findViewById(R.id.stub_comm_lv);  
        
        if ( listStub.getVisibility() == View.VISIBLE ) {  
            // 已经加载, 否则还没有加载  
        }  

// --------------------------------------------------------------------------------------------       
        
        / 方式二  
        ViewStub listStub2 = (ViewStub) findViewById(R.id.stub_import) ; 
        
        // 成员变量commLv2为空则代表未加载  
        if ( commLv2 == null ) {  
            // 加载评论列表布局, 并且获取评论ListView,inflate函数直接返回ListView对象  
          commLv2 = (ListView)listStub2.inflate();  
        } else {  
            // ViewStub已经加载  
        }  
```
注意：

* 1、判断是否已经加载过， 如果通过 `setVisibility` 来加载，那么通过判断可见性即可；如果通过 `inflate()` 来加载是不可以通过判断可见性来处理的，而需要使用方式2来进行判断。

* 2、`findViewById` 的问题，注意 `ViewStub` 中是否设置了 `inflatedId`，如果设置了则需要通过 `inflatedId` 来查找目标布局的根元素。

* 3、`ViewStub` 对象只可以 `Inflate` 一次，之后 `ViewStub` 对象会被置为空。

* 4、`ViewStub` 不支持 `merge` 标签，意味着你不能引入包含 `merge` 标签的布局到 `ViewStub` 中。

### **3、Merge**

> Merge 标签本质上是一个 `Activity`，里面有一个 `LinearLayout` 对象。

```
<merge xmlns:android="http://schemas.android.com/apk/res/android">  

    <ImageView    
        android:layout_width="fill_parent"   
        android:layout_height="fill_parent"   

        android:scaleType="center"  
        android:src="@drawable/golden_gate" />  

    <TextView  
        android:layout_width="wrap_content"   
        android:layout_height="wrap_content"   
        android:layout_marginBottom="20dip"  
        android:layout_gravity="center_horizontal|bottom"  
        android:padding="12dip"  
        android:background="#AA000000"  
        android:textColor="#ffffffff"  
        android:text="Golden Gate" />  

</merge> 
```

参考：

[Android布局优化之ViewStub、include、merge使用与源码分析 - Mr.Simple的专栏 - CSDN博客](https://blog.csdn.net/bboyfeiyu/article/details/45869393)














