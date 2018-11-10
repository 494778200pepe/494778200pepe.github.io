---
layout: post
title:  "Android动画--Animator"
date:   2018-11-10 13:49:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 Animator 』
---

> `Android12`之后，推出了`Animator`，如何使用？与`PropertyAnimator`有何关系和区别呢？

### **简单使用**
```
        mBlueBall.animate()//
                .alpha(0)//
                .y(mScreenHeight / 2).setDuration(1000)
                // need API 12
                .withStartAction(new Runnable() {
                    @Override
                    public void run() {
                        Log.e(TAG, "START");
                    }
                    // need API 16
                }).withEndAction(new Runnable() {

            @Override
            public void run() {
                Log.e(TAG, "END");
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        mBlueBall.setY(0);
                        mBlueBall.setAlpha(1.0f);
                    }
                });
            }
        }).start();
```  

### **走一走代码**              
```
    // 得到一个 ViewPropertyAnimator
    public ViewPropertyAnimator animate() {
        if (mAnimator == null) {
            mAnimator = new ViewPropertyAnimator(this);
        }
        return mAnimator;
    }
    
    // 添加一个alpha的动画
    public ViewPropertyAnimator alpha(float value) {
        // 内部针对各个属性，定义了相应的int值，用于标记
        // 用对应的静态int值，保存
        animateProperty(ALPHA, value);
        return this;
    }
    
    private void animateProperty(int constantName, float toValue) {
        // 获取当前值
        float fromValue = getValue(constantName);
        // 间隔
        float deltaValue = toValue - fromValue;
        animatePropertyBy(constantName, fromValue, deltaValue);
    }
    
    private void animatePropertyBy(int constantName, float startValue, float byValue) {
        // First, cancel any existing animations on this property
        if (mAnimatorMap.size() > 0) {
            Animator animatorToCancel = null;
            Set<Animator> animatorSet = mAnimatorMap.keySet();
            for (Animator runningAnim : animatorSet) {
                PropertyBundle bundle = mAnimatorMap.get(runningAnim);
                if (bundle.cancel(constantName)) {
                    // property was canceled - cancel the animation if it's now empty
                    // Note that it's safe to break out here because every new animation
                    // on a property will cancel a previous animation on that property, so
                    // there can only ever be one such animation running.
                    if (bundle.mPropertyMask == NONE) {
                        // the animation is no longer changing anything - cancel it
                        animatorToCancel = runningAnim;
                        break;
                    }
                }
            }
            if (animatorToCancel != null) {
                animatorToCancel.cancel();
            }
        }
        // 将属性的int值，起始值，间隔值，保存在 NameValuesHolder 中
        NameValuesHolder nameValuePair = new NameValuesHolder(constantName, startValue, byValue);
        // mPendingAnimations 一个ArrayList，保存各个属性及对应的动画参数
        mPendingAnimations.add(nameValuePair);
        // 取消之前的动画任务
        mView.removeCallbacks(mAnimationStarter);
        // 开始动画
        mView.postOnAnimation(mAnimationStarter);
    }
    
    // NameValuesHolder
    static class NameValuesHolder {
        int mNameConstant;
        float mFromValue;
        float mDeltaValue;
        NameValuesHolder(int nameConstant, float fromValue, float deltaValue) {
            mNameConstant = nameConstant;
            mFromValue = fromValue;
            mDeltaValue = deltaValue;
        }
    }
    
    private Runnable mAnimationStarter = new Runnable() {
        @Override
        public void run() {
            startAnimation();
        }
    };

    private void startAnimation() {
        if (mRTBackend != null && mRTBackend.startAnimation(this)) {
            return;
        }
        mView.setHasTransientState(true);
        // 新建一个ValueAnimator，用来获取动画的轨迹，可以添加差值器
        ValueAnimator animator = ValueAnimator.ofFloat(1.0f);
        // 获取保存的动画属性列表
        ArrayList<NameValuesHolder> nameValueList =
                (ArrayList<NameValuesHolder>) mPendingAnimations.clone();
        mPendingAnimations.clear();
        int propertyMask = 0;
        int propertyCount = nameValueList.size();
        for (int i = 0; i < propertyCount; ++i) {
            NameValuesHolder nameValuesHolder = nameValueList.get(i);
            // 取出动画属性的int值，统一保存在 propertyMask 中
            propertyMask |= nameValuesHolder.mNameConstant;
        }
        // 把将要执行的动画保存起来
        mAnimatorMap.put(animator, new PropertyBundle(propertyMask, nameValueList));
        if (mPendingSetupAction != null) {
            mAnimatorSetupMap.put(animator, mPendingSetupAction);
            mPendingSetupAction = null;
        }
        if (mPendingCleanupAction != null) {
            mAnimatorCleanupMap.put(animator, mPendingCleanupAction);
            mPendingCleanupAction = null;
        }
        if (mPendingOnStartAction != null) {
            mAnimatorOnStartMap.put(animator, mPendingOnStartAction);
            mPendingOnStartAction = null;
        }
        if (mPendingOnEndAction != null) {
            mAnimatorOnEndMap.put(animator, mPendingOnEndAction);
            mPendingOnEndAction = null;
        }
        animator.addUpdateListener(mAnimatorEventListener);
        animator.addListener(mAnimatorEventListener);
        if (mStartDelaySet) {
            animator.setStartDelay(mStartDelay);
        }
        if (mDurationSet) {
            animator.setDuration(mDuration);
        }
        if (mInterpolatorSet) {
            animator.setInterpolator(mInterpolator);
        }
        animator.start();
    }
    
    private static class PropertyBundle {
        int mPropertyMask;
        ArrayList<NameValuesHolder> mNameValuesHolder;

        PropertyBundle(int propertyMask, ArrayList<NameValuesHolder> nameValuesHolder) {
            mPropertyMask = propertyMask;
            mNameValuesHolder = nameValuesHolder;
        }
        
        ...
    }    
        
    
    // 动画的执行
    public void onAnimationUpdate(ValueAnimator animation) {
            // 取出 PropertyBundle
            PropertyBundle propertyBundle = mAnimatorMap.get(animation);
            if (propertyBundle == null) {
                // Shouldn't happen, but just to play it safe
                return;
            }

            boolean hardwareAccelerated = mView.isHardwareAccelerated();

            // alpha requires slightly different treatment than the other (transform) properties.
            // The logic in setAlpha() is not simply setting mAlpha, plus the invalidation
            // logic is dependent on how the view handles an internal call to onSetAlpha().
            // We track what kinds of properties are set, and how alpha is handled when it is
            // set, and perform the invalidation steps appropriately.
            boolean alphaHandled = false;
            if (!hardwareAccelerated) {
                mView.invalidateParentCaches();
            }
            float fraction = animation.getAnimatedFraction();
            int propertyMask = propertyBundle.mPropertyMask;
            if ((propertyMask & TRANSFORM_MASK) != 0) {
                mView.invalidateViewProperty(hardwareAccelerated, false);
            }
            // 拿到动画属性的列表
            ArrayList<NameValuesHolder> valueList = propertyBundle.mNameValuesHolder;
            if (valueList != null) {
                int count = valueList.size();
                for (int i = 0; i < count; ++i) {
                    NameValuesHolder values = valueList.get(i);
                    // 根据起始值，间隔值，配合当前的fraction，计算出当前值
                    float value = values.mFromValue + fraction * values.mDeltaValue;
                    if (values.mNameConstant == ALPHA) {
                        alphaHandled = mView.setAlphaNoInvalidation(value);
                    } else {
                        // 设置
                        setValue(values.mNameConstant, value);
                    }
                }
            }
            if ((propertyMask & TRANSFORM_MASK) != 0) {
                if (!hardwareAccelerated) {
                    mView.mPrivateFlags |= View.PFLAG_DRAWN; // force another invalidation
                }
            }
            // invalidate(false) in all cases except if alphaHandled gets set to true
            // via the call to setAlphaNoInvalidation(), above
            if (alphaHandled) {
                mView.invalidate(true);
            } else {
                mView.invalidateViewProperty(false, false);
            }
            if (mUpdateListener != null) {
                mUpdateListener.onAnimationUpdate(animation);
            }
        }
        
    // 根据属性的对应int值，设置相应的属性
    private void setValue(int propertyConstant, float value) {
        final View.TransformationInfo info = mView.mTransformationInfo;
        final RenderNode renderNode = mView.mRenderNode;
        switch (propertyConstant) {
            case TRANSLATION_X:
                renderNode.setTranslationX(value);
                break;
            case TRANSLATION_Y:
                renderNode.setTranslationY(value);
                break;
            case TRANSLATION_Z:
                renderNode.setTranslationZ(value);
                break;
            case ROTATION:
                renderNode.setRotation(value);
                break;
            case ROTATION_X:
                renderNode.setRotationX(value);
                break;
            case ROTATION_Y:
                renderNode.setRotationY(value);
                break;
            case SCALE_X:
                renderNode.setScaleX(value);
                break;
            case SCALE_Y:
                renderNode.setScaleY(value);
                break;
            case X:
                renderNode.setTranslationX(value - mView.mLeft);
                break;
            case Y:
                renderNode.setTranslationY(value - mView.mTop);
                break;
            case Z:
                renderNode.setTranslationZ(value - renderNode.getElevation());
                break;
            case ALPHA:
                info.mAlpha = value;
                renderNode.setAlpha(value);
                break;
        }
    }   
```     

至此，分析完毕，`Animator`仅仅是借用了`ValueAnimator`获取速度曲线，两者没啥关系。谷歌弄这个`Animator`干啥呢？应该是为了方便使用吧。         




















