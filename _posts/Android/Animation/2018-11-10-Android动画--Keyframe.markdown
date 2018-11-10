---
layout: post
title:  "Android动画--Keyframe"
date:   2018-11-10 11:31:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 Keyframe 』
---

### **简单使用**
```
                Keyframe keyframe1 = Keyframe.ofFloat(0f, 0f);//第一帧动画 动画完成度0的时候的值是0
                Keyframe keyframe2 = Keyframe.ofFloat(.5f, 200.0f);//第二帧动画 动画完成度0.5也就是一半的时候值是200
                Keyframe keyframe3 = Keyframe.ofFloat(1f, 0f);//第三帧动画 动画完成度1也就是动画结束的时候值是0.

                PropertyValuesHolder property = PropertyValuesHolder.ofKeyframe("translationX", keyframe1, keyframe2, keyframe3);
                ObjectAnimator objectAnimator = ObjectAnimator.ofPropertyValuesHolder(image, property);
                objectAnimator.start();
```























