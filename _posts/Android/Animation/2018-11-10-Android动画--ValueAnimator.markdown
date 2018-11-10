---
layout: post
title:  "Android动画--ValueAnimator"
date:   2018-11-10 10:46:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 ValueAnimator 』
---
> `ValueAnimator`是`ObjectAnimator`的父类，使用方法差不多，不过功能更强大，更灵活，并且不受`Object`自身约束。

### **简单使用**
```
                ValueAnimator animator0=ValueAnimator.ofFloat(0,100);
                animator0.setTarget(image);
                animator0.setDuration(1000).start();
                animator0.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation) {
                        Float value=(Float) animation.getAnimatedValue();
                        Log.d("pepe","value:"+value);
                    }
                });
```

























