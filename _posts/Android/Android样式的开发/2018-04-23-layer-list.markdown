---
layout: post
title:  "layer-list"
date:   2018-04-23 13:49:00 +0800
categories: Android
tags: Android样式的开发
author: pepe
description: 『 layer-list 』
---

[android里面layer-list中的inset和clip到底有什么作用 - CSDN博客](https://blog.csdn.net/candyguy242/article/details/44176503)

### **layer-list**

> 将多个 bitmap 或shape、selector按照顺序层叠起来

### **应用**

```
<?xml version="1.0" encoding="utf-8"?>    
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" >    
    
    <item>    
        <shape android:shape="rectangle" >    
            <solid android:color="@color/green" />    
        </shape>    
    </item>    
    
    <item android:bottom="6dp">    
        <shape android:shape="rectangle" >    
            <solid android:color="@color/white" />    
        </shape>    
    </item>    
  <!-- 第三层 通过叠层 左下右间距 画钩子 -->  
    <item    
        android:bottom="1dp"    
        android:left="1dp"    
        android:right="1dp">    
        <shape android:shape="rectangle" >    
            <solid android:color="@color/white" />    
        </shape>    
    </item>    
    
</layer-list> 
```
![EditText]({{ site.baseurl }}/assets/images/android/layer-list1.png)

### **layer-list 结合 bitmap**
```
<?xml version="1.0" encoding="utf-8"?>  
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">  
    <item>  
      <bitmap android:src="@drawable/android_red"  
        android:gravity="center" />  
    </item>  
    <item android:top="10dp" android:left="10dp">  
      <bitmap android:src="@drawable/android_green"  
        android:gravity="center" />  
    </item>  
    <item android:top="20dp" android:left="20dp">  
      <bitmap android:src="@drawable/android_blue"  
        android:gravity="center" />  
    </item>  
</layer-list>  
```

```
<ImageView  
    android:layout_height="wrap_content"  
    android:layout_width="wrap_content"  
    android:src="@drawable/layers" />  
```
![bitmap]({{ site.baseurl }}/assets/images/android/layer-list2.png)

### **糅合三个标签**
```
<selector xmlns:android="http://schemas.android.com/apk/res/android">  
    <item android:state_pressed="true">  
        <layer-list>  
            <item android:bottom="8.0dip">  
                <shape>  
                    <solid android:color="#ffaaaaaa" />  
                </shape>  
            </item>  
            <item>  
                <shape>  
                    <corners android:bottomLeftRadius="4.0dip" android:bottomRightRadius="4.0dip" android:topLeftRadius="1.0dip" android:topRightRadius="1.0dip" />  
  
                    <solid android:color="#ffaaaaaa" />  
  
                    <padding android:bottom="1.0dip" android:left="1.0dip" android:right="1.0dip" android:top="0.0dip" />  
                </shape>  
            </item>  
            <item>  
                <shape>  
                    <corners android:bottomLeftRadius="3.0dip" android:bottomRightRadius="3.0dip" android:topLeftRadius="1.0dip" android:topRightRadius="1.0dip" />  
  
                    <solid android:color="@color/setting_item_bgcolor_press" />  
                </shape>  
            </item>  
        </layer-list>  
    </item>  
    <item>  
        <layer-list>  
            <item android:bottom="8.0dip">  
                <shape>  
                    <solid android:color="#ffaaaaaa" />  
                </shape>  
            </item>  
            <item>  
                <shape>  
                    <corners android:bottomLeftRadius="4.0dip" android:bottomRightRadius="4.0dip" android:topLeftRadius="1.0dip" android:topRightRadius="1.0dip" />  
  
                    <solid android:color="#ffaaaaaa" />  
  
                    <padding android:bottom="1.0dip" android:left="1.0dip" android:right="1.0dip" android:top="0.0dip" />  
                </shape>  
            </item>  
            <item>  
                <shape>  
                    <corners android:bottomLeftRadius="3.0dip" android:bottomRightRadius="3.0dip" android:topLeftRadius="1.0dip" android:topRightRadius="1.0dip" />  
  
                    <solid android:color="@color/setting_item_bgcolor" />  
                </shape>  
            </item>  
        </layer-list>  
    </item>  
</selector>  
```

参考：

[Android之用layer-list，shape，selector画各种背景 - CSDN博客](https://blog.csdn.net/jenly121/article/details/50319005)

[Android开发：shape和selector和layer-list的（详细说明） - CSDN博客](https://blog.csdn.net/brokge/article/details/9713041)

[Android样式的开发:layer-list篇](http://keeganlee.me/post/android/20150909)



