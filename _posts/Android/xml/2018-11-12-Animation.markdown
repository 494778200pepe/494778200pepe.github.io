---
layout: post
title:  "Animation"
date:   2018-11-12 22:17:00 +0800
categories: Android
tags: xml
author: pepe
description: 『 Animation 』
---

### **Property Animation**
```
// res/animator/property_animator.xml
// In Java: R.animator.filename
// In XML: @[package:]animator/filename

<set
  android:ordering=["together" | "sequentially"]>

    <objectAnimator
        android:propertyName="string"
        android:duration="int"
        android:valueFrom="float | int | color"
        android:valueTo="float | int | color"
        android:startOffset="int"
        android:repeatCount="int"
        android:repeatMode=["repeat" | "reverse"]
        android:valueType=["intType" | "floatType"]/>

    <animator
        android:duration="int"
        android:valueFrom="float | int | color"
        android:valueTo="float | int | color"
        android:startOffset="int"
        android:repeatCount="int"
        android:repeatMode=["repeat" | "reverse"]
        android:valueType=["intType" | "floatType"]/>

    <set>
        ...
    </set>
</set>
```

```
AnimatorSet set = (AnimatorSet) AnimatorInflater.loadAnimator(myContext,
    R.animator.property_animator);
set.setTarget(myObject);
set.start();
```

### **View Animation**



#### **Tween animation**
```
// res/anim/hyperspace_jump.xml
// In Java: R.anim.filename
// In XML: @[package:]anim/filename

<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="@[package:]anim/interpolator_resource"
    android:shareInterpolator=["true" | "false"] >
    <alpha
        android:fromAlpha="float"
        android:toAlpha="float" />
    <scale
        android:fromXScale="float"
        android:toXScale="float"
        android:fromYScale="float"
        android:toYScale="float"
        android:pivotX="float"
        android:pivotY="float" />
    <translate
        android:fromXDelta="float"
        android:toXDelta="float"
        android:fromYDelta="float"
        android:toYDelta="float" />
    <rotate
        android:fromDegrees="float"
        android:toDegrees="float"
        android:pivotX="float"
        android:pivotY="float" />
    <set>
        ...
    </set>
</set>
```

```
ImageView image = (ImageView) findViewById(R.id.image);
Animation hyperspaceJump = AnimationUtils.loadAnimation(this, R.anim.hyperspace_jump);
image.startAnimation(hyperspaceJump);
```
#### **Frame animation**
```
// res/drawable/rocket.xml
// In Java: R.drawable.filename
// In XML: @[package:]drawable.filename

<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot=["true" | "false"] >
    <item
        android:drawable="@[package:]drawable/drawable_resource_name"
        android:duration="integer" />
</animation-list>
```

```
ImageView rocketImage = (ImageView) findViewById(R.id.rocket_image);
rocketImage.setBackgroundResource(R.drawable.rocket_thrust);

rocketAnimation = rocketImage.getBackground();
if (rocketAnimation instanceof Animatable) {
    ((Animatable)rocketAnimation).start();
}
```

### **Interpolators**

* `AccelerateDecelerateInterpolator`：`@android:anim/accelerate_decelerate_interpolator`

* `AccelerateInterpolator`：@android:anim/accelerate_interpolator`

* `AnticipateInterpolator`：@android:anim/anticipate_interpolator`

* `AnticipateOvershootInterpolator`：@android:anim/anticipate_overshoot_interpolator`

* `BounceInterpolator`：@android:anim/bounce_interpolator`

* `CycleInterpolator`：@android:anim/cycle_interpolator`

* `DecelerateInterpolator`：@android:anim/decelerate_interpolator`

* `LinearInterpolator`：@android:anim/linear_interpolator`

* `OvershootInterpolator`：@android:anim/overshoot_interpolator`
