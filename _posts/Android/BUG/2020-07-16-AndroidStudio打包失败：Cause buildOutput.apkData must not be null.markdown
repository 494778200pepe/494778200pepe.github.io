---
layout: post
title:  "AndroidStudio打包失败：Cause buildOutput.apkData must not be null"
date:   2020-07-16 16:57:00 +0800
categories: Android
tags: BUG
author: pepe
description: 『 AndroidStudio打包失败：Cause buildOutput.apkData must not be null 』
---


今天打开之前编写的Android项目，编译，运行，调试都没问题，但是到最后打包的时候就死活不成功，
试了很多方法，在网上也没找到解决的办法，网上说的方法都没有用，
最后我尝试打包debug版本，成功了，于是我怀疑的以前的打包文件问题，
于是把app/release目录下的apk都删掉，然后再进行签名打包，就成功了。
可能是因为升级了Androidstudio跟gradle版本的原因



参考：

[AndroidStudio打包失败：Cause: buildOutput.apkData must not be null - 简书](https://www.jianshu.com/p/9ae1dc6bba16)






