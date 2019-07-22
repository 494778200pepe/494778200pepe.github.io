---
layout: post
title:  "Gradle Android里的Gradle"
date:   2019-07-20 11:25:00 +0800
categories: Android
tags: Gradle
author: pepe
description: 『 Android里的Gradle 』
---

### **settings.gradle与build.gradle的本质**

大家有没有想过这样一个问题为什么settings.gradle可以include子模块，又为什么build.gradle可以写如下配置呢：
```
buildscript {
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.3.1'
    }
}

allprojects {
    repositories {
        google()
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```
其实我们在 `settings.gradle` 与 `build.gradle` 中看似配置的信息其实都是调用对应对象的方法或者脚本块设置对应信息。

对应对象是什么玩意？其实 `settings.gradle` 文件最终会被翻译为 `Settings` 对象，而 `build.gradle` 文件最终会被翻译为 `Project` 对象，`build.gradle` 文件对应的就是每个 project 的配置。

Settings 与 Project 在 Gradle 中都有对应的类，也就是说只有 Gradle 这个框架定义的我们才能用，至于 Settings 与 Project 都定义了什么我们只能查看其官方文档啊。

### **Settings对象**

比如Settings类中定义了include方法： 

![gradle1]({{ site.baseurl }}/assets/images/android/Gradle/gradle1.png)

include方法api说明为：

![gradle2]({{ site.baseurl }}/assets/images/android/Gradle/gradle2.png)

看到了吧，我们每一个配置都是调用对应对象的方法。

我还发现Settings类中定义了如下方法：

![gradle3]({{ site.baseurl }}/assets/images/android/Gradle/gradle3.png)

那我们在setting.gradle文件里面写上如下代码试一下：
![gradle4]({{ site.baseurl }}/assets/images/android/Gradle/gradle4.png)

```
include ':app', ':library1', ':library2'

def pro = findProject(':app')
println '----------------------------'
println pro.getPath()
println '----------------------------'
```
然后执行gradle assembleDebug命令编译我们的项目输出如下：



输出了app这个project的信息。



### **Project对象**













### **Gradle对象**


















参考：

[Gradle入门到实战(一) — 全面了解Gradle](https://mp.weixin.qq.com/s?__biz=Mzg2NzAwMjY4MQ==&mid=2247483789&idx=1&sn=4b3bb2ab721c8ed7e05f1e8b2e0fbf70&chksm=ce4371dbf934f8cd7c484e8c5356d299bbd5d7790ee11bb0da9725068fa8e4b895f87379949f&token=655420148&lang=zh_CN#rd)










