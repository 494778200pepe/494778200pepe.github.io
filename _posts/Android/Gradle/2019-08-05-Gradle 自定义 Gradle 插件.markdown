---
layout: post
title:  "Gradle 自定义 Gradle 插件"
date:   2019-08-05 16:35:00 +0800
categories: Android
tags: Gradle
author: pepe
description: 『 自定义 Gradle 插件 』
---

Gradle中插件可以分为两类：脚本插件和对象插件。

### **脚本插件**

> 脚本插件就是一个普通的 gradle 构建脚本，通过在一个 `config.gradle` 脚本中定义一系列的 `task`，另一个构建脚本 `app.gradle` 通过 `apply from:'config.gradle' `即可引用这个脚本插件。

首先在项目根目录下新建一个 config.gradle 文件，在该文件中定义所需的 task。
```
//config.gradle
project.task("showConfig") {
     doLast {
        println("$project.name:showConfig")
    }
}
```
然后在需要引用的 module 的构建脚本中引用 `config.gradle`,例如在 `app.gradle` 中,由于 `config.gradle` 建立在根目录下，与 app 这个模块平级，所以需要注意路径问题 `../config.gradle`。
```
//app.gradle
apply from: '../config.gradle'
```
就是这么简单，此时运行 gradle 构建即可执行 showConfig 这个 task，
```
C:\c_project\MyApplication\app>gradle showConfig
> Task :app:showConfig
app:showConfig
```

### **对象插件**

> 对象插件是指实现了 `org.gradle.api.Plugin` 接口的类。Plugin 接口需要实现 `void apply(T target)` 这个方法。该方法中的泛型指的是此 Plugin 可以应用到的对象，而我们通常是将其应用到 Project 对象上。

编写对象插件主要有三种方式：

* 1、直接在gradle脚本文件中

* 2、在buildSrc目录下

* 3、在独立的项目下










参考：

[一篇文章带你了解Gradle插件的所有创建方式](https://www.jianshu.com/p/5b99e3af4d6b)

[Gradle 实现自定义插件 - 小雨伞漂流记 - OSCHINA](https://my.oschina.net/ososchina/blog/2994131)

[android自定义gradle插件之当前项目使用-android,ios,h5-51CTO博客](https://blog.51cto.com/xuguohongai/2147597)

[Gradle新建插件buildSrc - 小妖666个人笔记 - CSDN博客](https://blog.csdn.net/weixin_38883338/article/details/90727880)

[在AndroidStudio中自定义Gradle插件 - huachao1001的专栏 - CSDN博客](https://blog.csdn.net/huachao1001/article/details/51810328)

[Gradle自定义插件以及发布方法 - 简书](https://www.jianshu.com/p/d1d7fd48ff0b)

gradle插件上传Jcenter与自建Maven私服 - pf_1308108803的博客 - CSDN博客
https://blog.csdn.net/pf_1308108803/article/details/78119591

 [Gradle入门到实战(一) — 全面了解Gradle](https://mp.weixin.qq.com/s?__biz=Mzg2NzAwMjY4MQ==&mid=2247483789&idx=1&sn=4b3bb2ab721c8ed7e05f1e8b2e0fbf70&chksm=ce4371dbf934f8cd7c484e8c5356d299bbd5d7790ee11bb0da9725068fa8e4b895f87379949f&token=655420148&lang=zh_CN#rd)