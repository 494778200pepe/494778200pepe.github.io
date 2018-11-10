---
layout: post
title:  "Android动画--ValueAnimator：抛物线"
date:   2018-11-10 10:55:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 ValueAnimator 』
---


```
                ValueAnimator valueAnimator2 = new ValueAnimator();
                valueAnimator2.setDuration(3000);
                valueAnimator2.setObjectValues(new PointF(0, 0));
                valueAnimator2.setInterpolator(new LinearInterpolator());
                //setEvaluator和在ofObject中传入TypeEvaluator是一样的
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
























