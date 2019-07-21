---
layout: post
title:  "Proguard混淆 proguard-android.txt"
date:   2019-07-17 14:11:00 +0800
categories: Android
tags: 混淆
author: pepe
description: 『 proguard-android.txt 』
---

一般情况下，Android 的 gradle 中都会默认写着：
```
proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro' 
```
这一行代码很多人不了解。它的意思是，指定了两个 Proguard rules 文件，一个是通过getDefaultProguardFile() 方法获得官方自带的混淆规则文件路径，另一个是与当前 gradle 相同目录下的 proguard-rules.pro 文件路径。

proguard-android.txt 这个文件的位置在：`${sdk.dir}/tools/proguard/proguard-android.txt`。
这个默认文件中帮我们声明了许多混淆规则内容，包括：keep 所有继承自 View 的类，keep 所有继承自 Activity 的类，keep 所有 JavascriptInterface、native 方法声明，以及 keep 一些注解了@Keep 的内容。

下面来读一下：
```
# This is a configuration file for ProGuard.
# http://proguard.sourceforge.net/index.html#manual/usage.html
#
# This file is no longer maintained and is not used by new (2.2+) versions of the
# Android plugin for Gradle. Instead, the Android plugin for Gradle generates the
# default rules at build time and stores them in the build directory.

-dontusemixedcaseclassnames
-dontskipnonpubliclibraryclasses
-verbose

# Optimization is turned off by default. Dex does not like code run
# through the ProGuard optimize and preverify steps (and performs some
# of these optimizations on its own).
-dontoptimize
-dontpreverify
# Note that if you want to enable optimization, you cannot just
# include optimization flags in your own project configuration file;
# instead you will need to point to the
# "proguard-android-optimize.txt" file instead of this one from your
# project.properties file.

-keepattributes *Annotation*
-keep public class com.google.vending.licensing.ILicensingService
-keep public class com.android.vending.licensing.ILicensingService

# For native methods, see http://proguard.sourceforge.net/manual/examples.html#native
-keepclasseswithmembernames class * {
    native <methods>;
}

# keep setters in Views so that animations can still work.
# see http://proguard.sourceforge.net/manual/examples.html#beans
-keepclassmembers public class * extends android.view.View {
   void set*(***);
   *** get*();
}

# We want to keep methods in Activity that could be used in the XML attribute onClick
-keepclassmembers class * extends android.app.Activity {
   public void *(android.view.View);
}

# For enumeration classes, see http://proguard.sourceforge.net/manual/examples.html#enumerations
-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

-keepclassmembers class * implements android.os.Parcelable {
  public static final android.os.Parcelable$Creator CREATOR;
}

-keepclassmembers class **.R$* {
    public static <fields>;
}

# The support library contains references to newer platform versions.
# Don't warn about those in case this app is linking against an older
# platform version.  We know about them, and they are safe.
-dontwarn android.support.**

# Understand the @Keep support annotation.
-keep class android.support.annotation.Keep

-keep @android.support.annotation.Keep class * {*;}

-keepclasseswithmembers class * {
    @android.support.annotation.Keep <methods>;
}

-keepclasseswithmembers class * {
    @android.support.annotation.Keep <fields>;
}

-keepclasseswithmembers class * {
    @android.support.annotation.Keep <init>(...);
}
```

参考：

[Android 高级混淆和代码保护技术 · Drakeet 的个人博客](https://blog.csdn.net/hqiangtai/article/details/76037244)

