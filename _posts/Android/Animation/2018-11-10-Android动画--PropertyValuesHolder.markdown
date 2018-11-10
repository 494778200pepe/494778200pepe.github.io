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

### **本质**
```
    // Activity
    PropertyValuesHolder pvh1=PropertyValuesHolder.ofFloat("translationX",300f);

    // PropertyValuesHolder
    public static PropertyValuesHolder ofFloat(String propertyName, float... values) {
        // 返回的是 PropertyValuesHolder 的内部类 FloatPropertyValuesHolder
        // 属性名和属性值保存在 FloatPropertyValuesHolder 里面
        return new FloatPropertyValuesHolder(propertyName, values);
    }

    // PropertyValuesHolder.FloatPropertyValuesHolder
    static class FloatPropertyValuesHolder extends PropertyValuesHolder {

        ...

        public FloatPropertyValuesHolder(String propertyName, float... values) {
            // 保存属性名
            super(propertyName);
            // 保存属性值
            setFloatValues(values);
        }
        
        ...
        
        @Override
        public void setFloatValues(float... values) {
            // 保存属性值 到 关键帧
            super.setFloatValues(values);
            // 强转得到关键帧
            mFloatKeyframes = (Keyframes.FloatKeyframes) mKeyframes;
        }
    }
    
    // PropertyValuesHolder
    public void setFloatValues(float... values) {
        // 保存属性值的类型
        mValueType = float.class;
        // 生成关键帧
        mKeyframes = KeyframeSet.ofFloat(values);
    }
    
    // KeyframeSet
    // 生成关键帧
    public static KeyframeSet ofFloat(float... values) {
        boolean badValue = false;
        int numKeyframes = values.length;
        // 最少两个关键帧
        FloatKeyframe keyframes[] = new FloatKeyframe[Math.max(numKeyframes,2)];
        if (numKeyframes == 1) {
            keyframes[0] = (FloatKeyframe) Keyframe.ofFloat(0f);
            keyframes[1] = (FloatKeyframe) Keyframe.ofFloat(1f, values[0]);
            if (Float.isNaN(values[0])) {
                badValue = true;
            }
        } else {
            keyframes[0] = (FloatKeyframe) Keyframe.ofFloat(0f, values[0]);
            for (int i = 1; i < numKeyframes; ++i) {
                keyframes[i] =
                        (FloatKeyframe) Keyframe.ofFloat((float) i / (numKeyframes - 1), values[i]);
                if (Float.isNaN(values[i])) {
                    badValue = true;
                }
            }
        }
        if (badValue) {
            Log.w("Animator", "Bad value (NaN) in float animator");
        }
        return new FloatKeyframeSet(keyframes);
    }
``` 

> 看完了上面的分析，可以知道无论是`ObjectAnimator`、`ValueAnimator`、`PropertyValuesHolder`,还是`Keyframe`，其实都是通过保存关键帧来达到保存动画数据的。每个`PropertyValuesHolder`都持有一个`keyframeSet`，`ObjectAnimator`和`ValueAnimator`则持有一个` HashMap<String, PropertyValuesHolder> mValuesMap`用来存放`PropertyValuesHolder`。
























