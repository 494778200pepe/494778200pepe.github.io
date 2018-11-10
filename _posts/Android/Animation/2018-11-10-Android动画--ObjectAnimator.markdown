---
layout: post
title:  "Android动画--ObjectAnimator"
date:   2018-11-10 10:09:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 ObjectAnimator 』
---

### **简单实用**
```
// ofFloat()方法的第一个参数表示动画操作的对象（可以是任意对象），
// 第二个参数表示操作对象的属性名字（只要是对象有的属性都可以），
// 第三个参数之后就是动画过渡值。当然过度值可以有一个到N个，
// 如果是一个值的话默认这个值是动画过渡值的结束值。如果有N个值，动画就在这N个值之间过渡。

ObjectAnimator.ofFloat(image, "translationX", 0, 350, 0).setDuration(2500).start();
```

### **当作ValueAnimator使用**
```
                ObjectAnimator  anim12 = ObjectAnimator//
                        .ofFloat(image, "zhy", 1.0F,  0.0F)//
                        .setDuration(500);//
                anim12.start();
                anim12.addUpdateListener(new ValueAnimator.AnimatorUpdateListener()
                {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation)
                    {
                        float cVal = (Float) animation.getAnimatedValue();
                        image.setAlpha(cVal);
                        image.setScaleX(cVal);
                        image.setScaleY(cVal);
                    }
               });
```

### **添加监听**
```
                ObjectAnimator anim13 = ObjectAnimator.ofFloat(image, "alpha", 0.5f);

                anim13.addListener(new Animator.AnimatorListener()
                {

                    @Override
                    public void onAnimationStart(Animator animation)
                    {
                        Log.e("pepe", "onAnimationStart");
                    }

                    @Override
                    public void onAnimationRepeat(Animator animation)
                    {
                        // TODO Auto-generated method stub
                        Log.e("pepe", "onAnimationRepeat");
                    }

                    @Override
                    public void onAnimationEnd(Animator animation)
                    {
                        Log.e("pepe", "onAnimationEnd");
                        ViewGroup parent = (ViewGroup) image.getParent();
                        if (parent != null)
                            parent.removeView(image);
                    }

                    @Override
                    public void onAnimationCancel(Animator animation)
                    {
                        // TODO Auto-generated method stub
                        Log.e("pepe", "onAnimationCancel");
                    }
                });
                anim13.start();
```

### **简单版监听**
```
                ObjectAnimator anim14 = ObjectAnimator.ofFloat(image, "alpha", 0.5f);
                anim14.addListener(new AnimatorListenerAdapter()
                {
                    @Override
                    public void onAnimationEnd(Animator animation)
                    {
                        Log.e("pepe", "onAnimationEnd");
                        ViewGroup parent = (ViewGroup) image.getParent();
                        if (parent != null)
                            parent.removeView(image);
                    }
                });
                anim14.start();
```

### **其他**
```
                anim12.pause();
                anim12.resume();
                anim12.reverse();
```                
                
























