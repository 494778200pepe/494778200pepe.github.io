---
layout: post
title:  "Android动画--自定义Animation：电视关闭效果"
date:   2018-11-09 15:30:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 自定义Animation 』
---

```
/**
 * 电视关闭效果的动画
 */
public class TVAnimation extends Animation {

    //缩放的中心点，设置为图片的中心即可
    private int mCenterWidth;
    private int mCenterHeight;

    @Override
    public void initialize(int width, int height, int parentWidth, int parentHeight) {
        super.initialize(width, height, parentWidth, parentHeight);
        // 设置默认时长
        setDuration(1000);
        // 动画结束后保留状态
        setFillAfter(true);
        // 设置默认插值器
        setInterpolator(new AccelerateInterpolator());
        mCenterWidth = width / 2;
        mCenterHeight = height / 2;
    }

    @Override
    protected void applyTransformation(float interpolatedTime, Transformation t) {
        Matrix matrix = t.getMatrix();
        matrix.postScale(1, 1 - interpolatedTime, mCenterWidth, mCenterHeight);
    }
}
```

























