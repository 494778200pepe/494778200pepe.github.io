---
layout: post
title:  "Gradle com.android.application"
date:   2019-08-02 11:27:00 +0800
categories: Android
tags: Gradle
author: pepe
description: 『 com.android.application 』
---

在 module 的 build.gradle 文件中，一般有 `apply plugin: 'com.android.application'` 或者 `apply plugin: 'com.android.library'`，表示引入插件。
在 Project 类中是没有 android{} 这个方法的。那么这个方法是谁提供的呢？点击进入，可以发现 android{} 是有一个 AppExtension 或者 LibraryExtension(extends TestExtension) 的对象提供的。

那么 AppExtension 和 LibraryExtension 又是什么呢？

[AppExtension](http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.AppExtension.html)

```
@NonNull
    @Override
    protected BaseExtension createExtension(
            @NonNull Project project,
            @NonNull Instantiator instantiator,
            @NonNull AndroidBuilder androidBuilder,
            @NonNull SdkHandler sdkHandler,
            @NonNull NamedDomainObjectContainer<BuildType> buildTypeContainer,
            @NonNull NamedDomainObjectContainer<ProductFlavor> productFlavorContainer,
            @NonNull NamedDomainObjectContainer<SigningConfig> signingConfigContainer,
            @NonNull ExtraModelInfo extraModelInfo) {
        return project.getExtensions()
                .create(
                        "android",
                        AppExtension.class,
                        project,
                        instantiator,
                        androidBuilder,
                        sdkHandler,
                        buildTypeContainer,
                        productFlavorContainer,
                        signingConfigContainer,
                        extraModelInfo);
    }
```

Android Gradle Plugin源码分析 - 简书
https://www.jianshu.com/p/11f030b2034f

[LibraryExtension](http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.LibraryExtension.html)
