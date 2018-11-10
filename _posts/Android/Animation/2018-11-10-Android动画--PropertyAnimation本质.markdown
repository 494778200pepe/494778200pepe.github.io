---
layout: post
title:  "Android动画--PropertyAnimation本质"
date:   2018-11-10 11:33:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 PropertyAnimation本质 』
---

### **PropertyValuesHolder本质**
```
    // Activity，从 PropertyValuesHolder 看起
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

### **ObjectAnimator本质**
``` 
    // Activity
    ObjectAnimator.ofFloat(image, "translationX", 0, 350, 0).setDuration(2500).start();

    // ObjectAnimator
    public static ObjectAnimator ofFloat(Object target, String propertyName, float... values) {
        // 保存动画执行对象和属性名
        ObjectAnimator anim = new ObjectAnimator(target, propertyName);
        // 保存属性值
        anim.setFloatValues(values);
        return anim;
    }
    
    private ObjectAnimator(Object target, String propertyName) {
        setTarget(target);
        setPropertyName(propertyName);
    }
    
    public void setPropertyName(@NonNull String propertyName) {
        // mValues could be null if this is being constructed piecemeal. Just record the
        // propertyName to be used later when setValues() is called if so.
        if (mValues != null) {
            // 将第一个的 key 替换掉
            PropertyValuesHolder valuesHolder = mValues[0];
            String oldName = valuesHolder.getPropertyName();
            valuesHolder.setPropertyName(propertyName);
            mValuesMap.remove(oldName);
            mValuesMap.put(propertyName, valuesHolder);
        }
        // 这里保存了属性名
        mPropertyName = propertyName;
        // New property/values/target should cause re-initialization prior to starting
        mInitialized = false;
    }
    
    public void setFloatValues(float... values) {
        if (mValues == null || mValues.length == 0) {
            // No values yet - this animator is being constructed piecemeal. Init the values with
            // whatever the current propertyName is
            if (mProperty != null) {
                setValues(PropertyValuesHolder.ofFloat(mProperty, values));
            } else {
                // 保存相关属性的关键帧到 PropertyValuesHolder
                setValues(PropertyValuesHolder.ofFloat(mPropertyName, values));
            }
        } else {
            super.setFloatValues(values);
        }
    }
    
    public void setValues(PropertyValuesHolder... values) {
        int numValues = values.length;
        mValues = values;
        mValuesMap = new HashMap<String, PropertyValuesHolder>(numValues);
        for (int i = 0; i < numValues; ++i) {
            PropertyValuesHolder valuesHolder = values[i];
            // 将 PropertyValuesHolder 保存到 mValuesMap 中
            mValuesMap.put(valuesHolder.getPropertyName(), valuesHolder);
        }
        // New property/values/target should cause re-initialization prior to starting
        mInitialized = false;
    }
``` 


> 看完了上面的分析，可以知道无论是`ObjectAnimator`、`ValueAnimator`、`PropertyValuesHolder`,还是`Keyframe`，其实都是通过保存关键帧来达到保存动画数据的。每个`PropertyValuesHolder`都持有一个相关属性的`keyframeSet`，`ObjectAnimator`和`ValueAnimator`则持有一个` HashMap<String, PropertyValuesHolder> mValuesMap`用来存放各个`PropertyValuesHolder`。
























