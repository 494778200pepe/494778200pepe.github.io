---
layout: post
title:  "Gradle AppExtension"
date:   2019-08-02 11:27:00 +0800
categories: Android
tags: Gradle
author: pepe
description: 『 AppExtension 』
---

在 build.gradle 中，我们通常都可以看到：
```
android {
    compileSdkVersion 27
    defaultConfig {
        applicationId "com.a1one.myapplication"
        minSdkVersion 18
        targetSdkVersion 27
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled true
            debuggable true  
        }        
    }
}
```
这里的 android  是从哪里来的呢？我们在 Project 的 API 里面，是没有看到 android() 这个方法的。
这就要从 `apply plugin: 'com.android.application'` 说起。
这一句的意思是依赖安卓插件。


跟踪 apply 方法，其实是进入到 AppPlugin 的 `apply` 的方法，我们可以看到内部实现是直接调用父类B asePlugin 的 `apply` 方法
```
protected void apply(@NonNull Project project) {
        checkPluginVersion();

        this.project = project;
        ExecutionConfigurationUtil.setThreadPoolSize(project);
        checkPathForErrors();
        checkModulesForErrors();

        ProfilerInitializer.init(project);
        threadRecorder = ThreadRecorder.get();

        ProcessProfileWriter.getProject(project.getPath())
                .setAndroidPluginVersion(Version.ANDROID_GRADLE_PLUGIN_VERSION)
                .setAndroidPlugin(getAnalyticsPluginType())
                .setPluginGeneration(GradleBuildProject.PluginGeneration.FIRST);

        threadRecorder.record(
                ...
                this::configureProject);

        threadRecorder.record(
                ...
                // 配置 extension 的阶段
                this::configureExtension);
        
        threadRecorder.record(
                ...
                this::createTasks);

        // Apply additional plugins
        for (String plugin : AndroidGradleOptions.getAdditionalPlugins(project)) {
            project.apply(ImmutableMap.of("plugin", plugin));
        }
    }
```

跟踪 `configureExtension`，
```
 private void configureExtension() {
        ...
        
        extension =
                createExtension(
                        project,
                        instantiator,
                        androidBuilder,
                        sdkHandler,
                        buildTypeContainer,
                        productFlavorContainer,
                        signingConfigContainer,
                        extraModelInfo);

        ...
    }
```
而 createExtension 是一个抽象方法，我们到 AppPlugin 里面看实现：
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
至此，我们是就知道 build.gradle 里面的 android 为什么可以用了。

[AppExtension](http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.AppExtension.html)

`apply plugin:  'com.android.library'` 对应的是 `LibraryExtension`。
[LibraryExtension](http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.LibraryExtension.html)

参考：

[Android Gradle Plugin源码分析 - 简书](https://www.jianshu.com/p/11f030b2034f)




