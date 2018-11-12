---
layout: post
title:  "Android动画--LayoutTransition"
date:   2018-11-12 14:10:00 +0800
categories: Android
tags: Animation
author: pepe
description: 『 LayoutTransition 』
---

> LayoutAnimation虽能实现ViewGroup的进入动画，但只能在创建时有效。在创建后，再往里添加控件就不会再有动画。在API 11后，又添加了两个能实现在创建后添加控件仍能应用动画的方法，分别是`android:animateLayoutChanges`属性和`LayoutTransition`类。

### **android:animateLayoutChanges属性**

> 在API 11之后，Android为了支持`ViewGroup`类控件，在添加和移除其中控件时自动添加动画，为我们提供了一个非常简单的属性：`android:animateLayoutChanges=[true/false]`,所有派生自`ViewGroup`的控件都具有此属性，只要在`XML`中添加上这个属性，就能实现添加/删除其中控件时，带有默认动画了。 

默认的进入动画就是向下部控件下移，然后新添控件透明度从0到1显示出来。默认的退出动画是控件透明度从1变到0消失，下部控件上移。

### **LayoutTransaction**

> 上面虽然在ViewGroup类控件XML中仅添加一行`android:animateLayoutChanges=[true]`即可实现内部控件添加删除时都加上动画效果。但却只能使用默认动画效果，而无法自定义动画。 
为了能让我们自定义动画，谷歌在API 11时，同时为我们引入了一个类`LayoutTransaction`。 

```
LayoutTransaction mTransitioner = new LayoutTransition();

//入场动画:view在这个容器中消失时触发的动画
ObjectAnimator animIn = ObjectAnimator.ofFloat(null, "rotationY", 0f, 360f,0f);
mTransitioner.setAnimator(LayoutTransition.APPEARING, animIn);
 
//出场动画:view显示时的动画
ObjectAnimator animOut = ObjectAnimator.ofFloat(null, "rotation", 0f, 90f, 0f);
mTransitioner.setAnimator(LayoutTransition.DISAPPEARING, animOut);
 
PropertyValuesHolder pvhLeft = PropertyValuesHolder.ofInt("left",0,100,0);
PropertyValuesHolder pvhTop = PropertyValuesHolder.ofInt("top",1,1);
Animator changeAppearAnimator
            = ObjectAnimator.ofPropertyValuesHolder(layoutTransitionGroup, pvhLeft,pvhBottom,pvhTop,pvhRight);
mTransitioner.setAnimator(LayoutTransition.CHANGE_APPEARING,changeAppearAnimator);
 
layoutTransitionGroup.setLayoutTransition(mTransitioner);


//    LayoutTransition.APPEARING 当一个View在ViewGroup中出现时，对此View设置的动画
//    LayoutTransition.CHANGE_APPEARING 当一个View在ViewGroup中出现时，对此View对其他View位置造成影响，对其他View设置的动画
//    LayoutTransition.DISAPPEARING  当一个View在ViewGroup中消失时，对此View设置的动画
//    LayoutTransition.CHANGE_DISAPPEARING 当一个View在ViewGroup中消失时，对此View对其他View位置造成影响，对其他View设置的动画
//    LayoutTransition.CHANGE 不是由于View出现或消失造成对其他View位置造成影响，然后对其他View设置的动画。
//    注意动画到底设置在谁身上，此View还是其他View。
```

注意事项： 

* 1、`LayoutTransition.CHANGE_APPEARING`和`LayoutTransition.CHANGE_DISAPPEARING`必须使用`PropertyValuesHolder`所构造的动画才会有效果，不然无效！也就是说使用`ObjectAnimator`构造的动画，在这里是不会有效果的！ 

* 2、在构造`PropertyValuesHolder`动画时，`”left”`、`”top”`属性的变动是必写的。如果不需要变动，则直接写为：
```
PropertyValuesHolder pvhLeft = PropertyValuesHolder.ofInt("left",0,0);
PropertyValuesHolder pvhTop = PropertyValuesHolder.ofInt("top",0,0);
```

* 3、在构造`PropertyValuesHolder`时，所使用的`ofInt`,`ofFloat`中的参数值，第一个值和最后一个值必须相同，不然此属性所对应的的动画将被放弃，在此属性值上将不会有效果；
```
PropertyValuesHolder pvhLeft = PropertyValuesHolder.ofInt("left",0,100,0);
```

比如，这里`ofInt(“left”,0,100,0)`第一个值和最后一个值都是0，所以这里会有效果的，如果我们改为`ofInt(“left”,0,100);`那么由于首尾值不一致，则将被视为无效参数，将不会有效果！ 

* 4、在构造`PropertyValuesHolder`时，所使用的`ofInt`,`ofFloat`中，如果所有参数值都相同，也将不会有动画效果。 
比如：
```
PropertyValuesHolder pvhLeft = PropertyValuesHolder.ofInt("left",100,100);
```


参考：

[自定义控件三部曲之动画篇(十二)——animateLayoutChanges与LayoutTransition - 启舰 - CSDN博客](https://blog.csdn.net/harvic880925/article/details/50985596)













