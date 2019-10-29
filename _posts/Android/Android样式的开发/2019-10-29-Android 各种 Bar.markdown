---
layout: post
title:  "Android 各种 Bar"
date:   2019-10-29 09:35:00 +0800
categories: Android
tags: Android样式的开发
author: pepe
description: 『 各种 Bar 』
---

![bar1]({{ site.baseurl }}/assets/images/android/theme/bar1.png)

### **StatusBar**

StatusBar,也就是状态栏，它处于屏幕的最顶部，正常情况下它是显示的，它和TitleBar和ActionBar、ToolBar之间没有直接的关系。
可设置隐藏、颜色，获取高度等。

### **TitleBar**
TitleBar，也就是标题栏,它紧挨状态栏的下面，正常情况下它的布局和主题样式都是使用系统定义好的，且默认情况下只显示图标和文本。

### **ActionBar**
ActionBar 是android 3.0的推出的，当时Google 想要逐渐改善过去 android 纷乱的界面设计，希望让终端使用者尽可能在 android 手机有个一致的操作体验。
可设置标题、图标、样式、按钮、menu等。

> Action bar被包含在所有的使用Theme.Hole主题的Activity（或者是这些Activity的子类）中。

开发API11以下的程序，首先必须在AndroidManifest.xml中指定Application或Activity的theme是Theme.Holo或其子类，否则将无法使用ActionBar。

#### 删除actionbar

* 方法一：
	
	。如果不想用ActionBar，那么只要在theme主题后面" .NoActionBar", 就可以了。

* 方法二：

	。在onCreate方法中添加一句代码: requestWindowFeature(Window.FEATURE_NO_TITLE);
	。不过这句代码一定要添加到setContentView(R.layout.activity_main); 之前


* 方法三：

	。用getActionBar()/getSupportActionBar()得到ActionBar对象，用对象调用hide()方法；
	。注意配置清单文件中最低版本改为11以上；
	

```
public class MainActivity extends Activity {
    ActionBar  actionBar;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
 
        setContentView(R.layout.activity_main);
        actionBar=getActionBar();
        actionBar.hide();
    }
}
```

### **ToolBar**

> Toolbar 是android 5.0的推出的，放在了v7包中作为控件，它是为了取代actionbar而产生的.由于ActionBar在各个安卓版本和定制Rom中的效果表现不一，导致严重的碎片化问题，ToolBar应运而生。

优点：自定义视图的操作更加简单，状态栏的颜色可以调（Android 4.4以上）。
setSupportActionBar ，Toolbar即能取代原本的 actionbar 了.

```
Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
setSupportActionBar(toolbar);
```


### **NavagationBar**

Android设备的虚拟导航栏，早年间的Android设备都还是物理导航栏。随着Android设备的进步（各种搞事情），出现了虚拟导航栏。由于考虑到降低屏占比，又出现了全面屏，可以控制导航栏的开关。

![bar2]({{ site.baseurl }}/assets/images/android/theme/bar2.png)

参考:

[ActionBar、TitleBar、ToolBar、StatusBar之间的关系 - LuckyDucky的博客 - CSDN博客](https://blog.csdn.net/sinat_29675423/article/details/86254222)

































