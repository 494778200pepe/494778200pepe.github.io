---
layout: post
title:  "无线连接ADB调试Android设备"
date:   2018-07-25 16:07:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 无线连接ADB调试Android设备 』
---

### **查看设备IP**

手机连接到电脑的时候，输入 adb shell ip -f inet addr show wlan0 即可看到手机的IP。

![step1]({{ site.baseurl }}/assets/images/android/adb_step1.png)

### **设置端口号**

手机连接到电脑，输入以下指令`adb tcpip {端口号}` ，端口号比如5555，后面用这个端口连接。

![step2]({{ site.baseurl }}/assets/images/android/adb_step2.png)

### **连接设备**

拔掉USB线，输入`adb connect {设备IP}:{端口号} `，设备IP就是手机的IP，要保证和电脑在一个局域网内，端口号就是第一步的端口号。无意外的话，已经链接上了。

![step3]({{ site.baseurl }}/assets/images/android/adb_step3.png)

参考：

[无线连接ADB调试Android设备 | Kyleduo's blog!](https://blog.kyleduo.com/2016/11/11/adb-via-wifi/)