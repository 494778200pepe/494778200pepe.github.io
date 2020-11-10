---
layout: post
title:  "ButterKnife 手写"
date:   2018-07-17 16:15:00 +0800
categories: Android
tags: 开源框架
author: pepe
description: 『 手写 』
---

### 错误纪录一

```
AS报错：Invoke-customs are only supported starting with Android O (--min-api 26)


compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
}

```

### 错误纪录二

```
implementation 'com.google.auto.service:auto-service:1.0-rc6'
//gradle3.0以上版本需要
annotationProcessor 'com.google.auto.service:auto-service:1.0-rc6' 
```
