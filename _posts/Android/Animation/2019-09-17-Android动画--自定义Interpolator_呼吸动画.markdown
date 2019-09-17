---
layout: post
title:  "Android动画--自定义Interpolator_呼吸动画"
date:   2019-09-17 16:53:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 自定义Interpolator_呼吸动画 』
---

```
 
import android.animation.TimeInterpolator;
 
/**
 * 定义拟合呼吸变化的插值器
 */
 
public class BreatheInterpolator implements TimeInterpolator {
    @Override
    public float getInterpolation(float input) {
 
        float x = 6 * input;
        float k = 1.0f / 3;
        int t = 6;
        int n = 1;//控制函数周期，这里取此函数的第一个周期
        float PI = 3.1416f;
        float output = 0;
 
        if (x >= ((n - 1) * t) && x < ((n - (1 - k)) * t)) {
            output = (float) (0.5 * Math.sin((PI / (k * t)) * ((x - k * t / 2) - (n - 1) * t)) + 0.5);
 
        } else if (x >= (n - (1 - k)) * t && x < n * t) {
            output = (float) Math.pow((0.5 * Math.sin((PI / ((1 - k) * t)) * ((x - (3 - k) * t / 2) - (n - 1) * t)) + 0.5), 2);
        }
        return output;
    }
}


  /**
     * 开启透明度渐变呼吸动画
     */
    private void startAlphaBreathAnimation() {
        ObjectAnimator alphaAnimator = ObjectAnimator.ofFloat(tvShowBreathing, "alpha", 0f, 1f);
        alphaAnimator.setDuration(4000);
        alphaAnimator.setInterpolator(new BreatheInterpolator());//使用自定义的插值器
        alphaAnimator.setRepeatCount(ValueAnimator.INFINITE);
        alphaAnimator.start();
    }

```

参考：

[更加自然的渐变——呼吸动画 - 同中书门下平章事jaren的博客 - CSDN博客](https://blog.csdn.net/l_wwbs/article/details/54691770)























