---
layout: post
title:  "android studio离线配置gradle"
date:   2020-07-07 15:41:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 android studio离线配置gradle 』
---


以windows为例，整个目录应该在C:\Users\Administrator\.gradle\wrapper\dists下面。

假如我们缺少某gradle版本：gradle-xxx-all，那么studio会开始下载，很慢，所以推荐使用离线下载。

去http://www.androiddevtools.cn/，顶部第二栏，dev tool下载gradle对应压缩包，下载到本地。

studio开始下载并在文件夹下创建gradle文件夹，以及下面还有个很长的随机字符的文件夹，把压缩包复制到这个文件夹中，关闭studio再打开就可以了。



一般以上操作都可以解决问题！但是！我就遇到了无法解决问题的例外。

压缩包放进去了，as还是继续在慢慢的下载。但是路径感觉又没有错。

解决方式：

此时在随机字符文件夹底下有了个gradle-xxxx文件夹，点进去\wrapper\dists\gradle-xxxx-all，你会发现底下又有一个随机字符文件夹！！

这个时候你只要把压缩包放在这里，并删除同级gradle-xxxx文件夹，重启as就可以了。



参考：

[android studio离线配置gradle_h309849232的博客-CSDN博客_android studio配置离线编译](https://blog.csdn.net/h309849232/article/details/74343753)