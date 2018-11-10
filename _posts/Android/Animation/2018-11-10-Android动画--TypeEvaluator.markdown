---
layout: post
title:  "Android动画--TypeEvaluator"
date:   2018-11-10 13:21:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 TypeEvaluator 』
---

> `TypeEvaluator`，估值器。什么东西？`Interpolator`是用来控制`fraction`的变化曲线的。`TypeEvaluator`则是用来将`fraction`转换成实际需要的值(可能是点，可能是颜色值 等等)的。

```
                ValueAnimator valueAnimator2 = new ValueAnimator();
                valueAnimator2.setDuration(3000);
                // 设置关键帧
                valueAnimator2.setObjectValues(new PointF(0, 0));
                valueAnimator2.setInterpolator(new LinearInterpolator());
                //setEvaluator和在ofObject中传入TypeEvaluator是一样的
                // 添加估值器，fraction从0-1，Evaluator计算得到需要的值
                valueAnimator2.setEvaluator(new TypeEvaluator<PointF>()
                {
                    // fraction = t / duration
                    @Override
                    public PointF evaluate(float fraction, PointF startValue,
                                           PointF endValue)
                    {
                        Log.e("pepe", fraction * 3 + "");
                        // x方向200px/s ，则y方向0.5 * 10 * t
                        PointF point = new PointF();
                        point.x = 200 * fraction * 3;
                        point.y = 0.5f * 200 * (fraction * 3) * (fraction * 3);
                        return point;
                    }
                });

                valueAnimator2.start();
                valueAnimator2.addUpdateListener(new ValueAnimator.AnimatorUpdateListener()
                {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation)
                    {
                        PointF point = (PointF) animation.getAnimatedValue();
                        image.setX(point.x);
                        image.setY(point.y);

                    }
                });
```




















