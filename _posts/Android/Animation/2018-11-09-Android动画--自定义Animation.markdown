---
layout: post
title:  "Android动画--自定义Animation"
date:   2018-11-09 15:26:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 自定义Animation 』
---

> 通过重写`Animation`的`applyTransformation (float interpolatedTime, Transformation t)`来实现自定义动画效果，另外一般也会实现`initialize (int width, int height, int parentWidth, int parentHeight)`，这是一个回调方法告诉`Animation`目标View的大小参数，在这里可以初始化一些相关的参数，例如设置动画持续时间、设置`Interpolator`、设置动画的参考点等。在绘制动画的过程中会反复的调用`applyTransformation()` ，每次调用参数`interpolatedTime`值都会变化，该参数从0渐变为1，当该参数为1时表明动画结束。通过参数`Transformation`来获取变换的矩阵(`matrix`)，通过改变矩阵就可以实现各种复杂的效果。

例子一：
```
/**
 * 3D动画效果
 */
public class CustomAnimation extends Animation {

    private int mCenterWidth;
    private int mCenterHeight;
    private float mRotateY = 30;
    private Camera mCamera = new Camera();

    @Override
    public void initialize(int width, int height, int parentWidth, int parentHeight) {
        super.initialize(width, height, parentWidth, parentHeight);
        // 设置默认时长
        setDuration(2000);
        // 动画结束后保留状态
        setFillAfter(true);
        // 设置默认插值器
        setInterpolator(new BounceInterpolator());
        mCenterWidth = width / 2;
        mCenterHeight = height / 2;
    }

    @Override
    protected void applyTransformation(float interpolatedTime, Transformation t) {
        Matrix matrix = t.getMatrix();
        mCamera.save();
        // 使用Camera设置旋转的角度
        mCamera.rotateY(interpolatedTime * mRotateY);
        // 将旋转变换作用到matrix上
        mCamera.getMatrix(matrix);
        mCamera.restore();
        // 通过pre方法设置矩阵作用前的偏移量来改变旋转中心
        matrix.preTranslate(mCenterWidth, mCenterHeight);
        matrix.postTranslate(-mCenterWidth, -mCenterHeight);
    }

    // 暴露接口-设置旋转角度
    public void setRotateY(float rotateY) {
        mRotateY = rotateY;
    }
}
```


例子二：
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
参考：

[Android 自定义Animation动画 - 夏天的风的日志 - 网易博客](http://longshuai2007.blog.163.com/blog/static/1420944142011719103059746/)

























