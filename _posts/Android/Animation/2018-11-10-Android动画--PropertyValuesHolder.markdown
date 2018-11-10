---
layout: post
title:  "Android动画--PropertyValuesHolder"
date:   2018-11-10 11:07:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 PropertyValuesHolder 』
---

### **简单使用**
```
                PropertyValuesHolder pvh1=PropertyValuesHolder.ofFloat("translationX",300f);
                PropertyValuesHolder pvh2=PropertyValuesHolder.ofFloat("scaleX",1f,0.5f);
                PropertyValuesHolder pvh3=PropertyValuesHolder.ofFloat("scaleY",1f,0.5f);
                ObjectAnimator.ofPropertyValuesHolder(image,pvh1,pvh2,pvh3).setDuration(1000).start();
```
























