---
layout: post
title:  "Android动画--自定义Interpolator"
date:   2018-11-09 14:27:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 自定义Interpolator 』
---

先来看看差值器的源码

### **LinearInterpolator**

```
/**
 * An interpolator where the rate of change is constant
 */
@HasNativeInterpolator
public class LinearInterpolator extends BaseInterpolator implements NativeInterpolatorFactory {

    public LinearInterpolator() {
    }

    public LinearInterpolator(Context context, AttributeSet attrs) {
    }

    public float getInterpolation(float input) {
        return input;
    }

    /** @hide */
    @Override
    public long createNativeInterpolator() {
        return NativeInterpolatorFactoryHelper.createLinearInterpolator();
    }
}
```
### **AccelerateDecelerateInterpolator**

```
/**
 * An interpolator where the rate of change starts and ends slowly but
 * accelerates through the middle.
 * 在动画开始与结束的地方速率改变比较慢，在中间的时候加速
 */
@HasNativeInterpolator
public class AccelerateDecelerateInterpolator extends BaseInterpolator
        implements NativeInterpolatorFactory {
    public AccelerateDecelerateInterpolator() {
    }

    @SuppressWarnings({"UnusedDeclaration"})
    public AccelerateDecelerateInterpolator(Context context, AttributeSet attrs) {
    }

    public float getInterpolation(float input) {
        return (float)(Math.cos((input + 1) * Math.PI) / 2.0f) + 0.5f;
    }

    /** @hide */
    @Override
    public long createNativeInterpolator() {
        return NativeInterpolatorFactoryHelper.createAccelerateDecelerateInterpolator();
    }
}
```

看了上面的源码，相信你应该会知道要如何做了。

* 继承`BaseInterpolator`，重写`getInterpolation()`
* `input`表示的是当前动画执行的时间点，范围是0f -1f


```
// 一个先减速后加速的差值器
public class DecelerateAccelerateInterpolator implements TimeInterpolator{  

    public DecelerateAccelerateInterpolator() {
    }

    @SuppressWarnings({"UnusedDeclaration"})
    public DecelerateAccelerateInterpolator(Context context, AttributeSet attrs) {
    }
  
    @Override  
    public float getInterpolation(float input) {  
        float result;  
        if (input <= 0.5) {  
            result = (float) (Math.sin(Math.PI * input)) / 2;  
        } else {  
            result = (float) (2 - Math.sin(Math.PI * input)) / 2;  
        }  
        return result;  
    }  
  
} 
```
























