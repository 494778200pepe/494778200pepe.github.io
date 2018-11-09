---
layout: post
title:  "Android动画--Frame Animation"
date:   2018-11-09 09:25:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 Frame Animation 』
---

> 在Android3.0以前，官方只提供了两种动画Frame Animation(帧动画)和Tween Animation(补间动画)，统称为View Animation(视图动画)。

下面来介绍Frame Animation

### **xml文件编写**
```
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="true">
    <item android:drawable="@mipmap/flower1" android:duration="500"/>
    <item android:drawable="@mipmap/flower2" android:duration="500"/>
    <item android:drawable="@mipmap/flower3" android:duration="500"/>
    <item android:drawable="@mipmap/flower4" android:duration="500"/>
    <item android:drawable="@mipmap/flower5" android:duration="500"/>
    <item android:drawable="@mipmap/flower6" android:duration="500"/>
</animation-list>

<!--Android:onshot如果定义为true的话，此动画只会执行一次，如果为false则一直循环。-->
```

### **开始动画：方式一**
```
//将动画资源文件设置为ImageView的背景
image.setBackgroundResource(R.drawable.frameanim);  
//获取ImageView背景,此时已被编译成AnimationDrawable
AnimationDrawable anim = (AnimationDrawable) image.getBackground(); 
//开始动画
anim.start();   
```

注意：开始动画的执行不能放在`onCreate()`中，因为那时候窗口`Window`对象还没有完全初始化，`AnimationDrawable`不能完全追加到窗口`Window`对象中，那么该怎么办呢？我们需要把这段代码放在`onWindowFocusChanged()`中，当`Activity`展示给用户时，`onWindowFocusChanged()`就会被调用，我们正是在这个时候实现我们的动画效果。

### **开始动画：方式二**
```
// 通过逐帧动画的资源文件获得AnimationDrawable示例
AnimationDrawable frameAnim = (AnimationDrawable) getResources().getDrawable(R.drawable.frameanim);
// 把AnimationDrawable设置为ImageView的背景
image.setBackgroundDrawable(frameAnim);
frameAnim.start();
```

### **开始动画：方式三**
```
//完全编码实现的动画效果
AnimationDrawable anim = new AnimationDrawable();
for (int i = 1; i <= 6; i++) {
    //根据资源名称和目录获取R.java中对应的资源ID
    int id = getResources().getIdentifier("flower" + i, "mipmap", getPackageName());
    //根据资源ID获取到Drawable对象
    Drawable drawable = getResources().getDrawable(id,null);
    //将此帧添加到AnimationDrawable中
    anim.addFrame(drawable, 500);
}
anim.setOneShot(false); //设置为loop
image.setBackground(anim);  //将动画设置为ImageView背景
anim.start();   //开始动画
```

### **停止动画**
```
AnimationDrawable anim = (AnimationDrawable) image.getBackground();
if (anim.isRunning()) {	
    //如果正在运行,就停止
	anim.stop();
}
```

### 其他

`animation-list`还有两个自定义属性：
```
android:visible 参数为布尔值，设置AnimationDrawable的可见性，true可见，false不可见，xml中定义的visible属性无用，因为根本没有解析。
android:variablePadding 表示是否支持可变的Padding。false表示使用所有帧中最大的Padding，true表示使用当前帧的padding。
```


参考：

[详解Android动画之Frame Animation - LiuHe - CSDN博客](https://blog.csdn.net/liuhe688/article/details/6657776)

[Android动画总结系列（1）——帧动画 - u013478336的专栏 - CSDN博客](https://blog.csdn.net/u013478336/article/details/52137385)