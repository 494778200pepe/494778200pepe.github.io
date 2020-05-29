---
layout: post
title:  "SDK Version"
date:   2020-01-09 14:38:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 SDK Version 』
---

Gradle 版本：`C:\Users\Administrator\.gradle\wrapper\dists`

![version1]({{ site.baseurl }}/assets/images/android/sdk_version/sdk_version1.png)

compileSdkVersion:编译SDK的版本 `C:\install\SDK\platforms`

![version2]({{ site.baseurl }}/assets/images/android/sdk_version/sdk_version2.png)

buildToolsVersion:构建工具的版本:`C:\install\SDK\build-tools`

![version3]({{ site.baseurl }}/assets/images/android/sdk_version/sdk_version3.png)

targetSdkVersion:目标设备的sdk版本



* 1、CompileSdkVersion是你SDK的版本号，也就是API Level，例如API-19、API-20、API-21等等。

* 2、buildeToolVersion是你构建工具的版本，其中包括了打包工具aapt、dx等等。这个工具的目录位于..your_sdk_path/build-tools/XX.XX.XX

	这个版本号一般是API-LEVEL.0.0。例如I/O2014大会上发布了API20对应的build-tool的版本就是20.0.0

	在这之间可能有小版本，例如20.0.1等等。

* 3、可以用高版本的build-tool去构建一个低版本的sdk工程，例如build-tool的版本为20，去构建一个sdk版本为18的

	例如：compileSdkVersion 18  

	buildToolsVersion "22.0.1"这样也是OK的。
	
* 4、com.android.support:appcompat-v7 版本号问题

	supportLibVersion 的头数字是和targetSdkVersion 版本一样的。

关键目录：

* platforms：是存在不同API-LEVEL版本SDK目录的地方

* build-tools:里面是不同版本的build工具，这些工具包括了aapt打包工具、dx、aidl等。

* platform-tools：是一些Android平台相关的工具，如adb、fastboot、sqlite3等

* tools:是存放一些Android开发相关的工具，如android、emulator、monitor、traceview


