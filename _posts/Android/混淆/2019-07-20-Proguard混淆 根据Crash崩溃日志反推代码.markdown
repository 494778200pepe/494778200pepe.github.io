---
layout: post
title:  "Proguard混淆 根据Crash崩溃日志反推代码"
date:   2019-07-20 10:50:00 +0800
categories: Android
tags: 混淆
author: pepe
description: 『 根据Crash崩溃日志反推代码 』
---

混淆成功后，除生成了指定类型的混淆包外，还会在工程的根目录下或是根目录下得 bin 文件夹中生成 proguard 文件夹，里面包含 `dump.txt`、`mapping.txt`、`seeds.txt` 和 `usage.txt` 四个文件。

* `dump.txt`:描述.apk文件中所有原始类文件间的内部结构

* `mapping.txt`:列出了原始的类，方法和字段名与混淆后代码间的映射。这个文件很重要，当你从 release 版本中收到一个 bug 报告时，可以用它来翻译被混淆的代码。

* `seeds.txt`:列出了未被混淆的类和成员

* `usage.txt`:列出了从 .apk 中删除的代码

### **如何还原 ProGuard 混淆后的代码**

ProGuard 在 Android 应用发布的时候经常会用来混淆代码。

混淆后的应用发布到市场上，当用户反馈 Crash 的时候， 开发者看起来就不那么好定位问题根源了。

例如：
```
Caused by: java.lang.NullPointerException
at net.simplyadvanced.ltediscovery.be.u(Unknown Source)
at net.simplyadvanced.ltediscovery.at.v(Unknown Source)
at net.simplyadvanced.ltediscovery.at.d(Unknown Source)
at net.simplyadvanced.ltediscovery.av.onReceive(Unknown Source)
```
还好 ProGuard 本身提供了一个还原工具。 

要使用还原工具之前，你需要 **保存每次发布应用混淆后的 proguard/mapping.txt 文件**。

在每次用 ProGuard 发布应用的时候， 都会在项目目录下的 proguard 目录中创建新的 mapping 文件。

**该文件记录了 每个类对应混淆后的类以及方法。**

还原后的代码就比较好理解了：
```
Caused by: java.lang.NullPointerException
at net.simplyadvanced.ltediscovery.UtilTelephony.booleanis800MhzNetwork()(Unknown Source)
at net.simplyadvanced.ltediscovery.ServiceDetectLte.voidcheckAndAlertUserIf800MhzConnected()(Unknown Source)
at net.simplyadvanced.ltediscovery.ServiceDetectLte.voidstartLocalBroadcastReceiver()(Unknown Source)
at net.simplyadvanced.ltediscovery.ServiceDetectLte$2.voidonReceive(android.content.Context,android.content.Intent)(Unknown Source)
```
ProGuard 提供了命令行和 GUI 工具来还原混淆后的代码。

该工具位于 `<android-sdk>/tools/proguard/bin/` 目录下。

里面的 **proguardgui.bat 为 GUI 工具**，
```
1) 运行 proguardgui.bat
2) 从左边的菜单选择  “ReTrace”
3) 在上面的 mapping 文件中选择你的 mapping 文件 ，在下面输入框输入要还原的代码
4) 点击 “ReTrace!” 按钮
```

#### retrace.bat 为命令行工具， 

把 「mapping 文件」和 「要还原的堆栈信息保存在 stacktrace 文件中，

然后把这两个文件复制到 「retrace.bat 目录「下，运行如下命令即可。

> `retrace.bat -verbose mapping.txt stacktrace.txt > out.txt`






参考：

[Android 实现一键反混淆功能 - laughing_lh的专栏 - CSDN博客](https://blog.csdn.net/laughing_lh/article/details/82796898)

[AVA之代码混淆proguard基础(三)从异常堆栈中还原 ProGuard 混淆过的代码 - FastThinking的专栏 - CSDN博客](https://blog.csdn.net/FastThinking/article/details/49420467)












