---
layout: post
title:  "Android动画--AnimatorSet"
date:   2018-11-10 13:46:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 AnimatorSet 』
---

### **简单使用**
```
                ObjectAnimator animator2=ObjectAnimator.ofFloat(image,"translationX",300f);
                ObjectAnimator animator3=ObjectAnimator.ofFloat(image,"scaleX",1f,0f,1f);
                ObjectAnimator animator4=ObjectAnimator.ofFloat(image,"scaleY",1f,0f,1f);
                AnimatorSet set=new AnimatorSet();
                set.setDuration(1000);
                set.playTogether(animator2,animator3,animator4);
                set.start();
```                
                


### **xml实现**

`<set />:对应AnimatorSet`
















