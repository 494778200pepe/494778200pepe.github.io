---
layout: post
title:  "如何查看Android设备的CPU架构"
date:   2018-07-30 10:44:00 +0800
categories: Android
tags: NDK
author: pepe
description: 『 如何查看Android设备的CPU架构 』
---

### **常见CPU架构**
```
'x86','x86_64','armeabi','armeabi-v7a','arm64-v8a'
```

### **通过命令查看**
```
1、adb shell  
2、cat /proc/cpuinfo
```

可以从输出看到ARMv7
```
Processor : ARMv7 Processor rev 0 (v7l) 
processor : 0 
BogoMIPS : 38.40 
processor : 1 
BogoMIPS : 38.40 
processor : 2 
BogoMIPS : 38.40 
processor : 3 
BogoMIPS : 38.40 
Features : swp half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt 
CPU implementer : 0x51 
CPU architecture: 7 
CPU variant : 0x2 
CPU part : 0x06f 
CPU revision : 0 
Hardware : Qualcomm MSM 8974 HAMMERHEAD (Flattened Device Tree) 
Revision : 000b 
Serial : 0000000000000000
```

### **通过代码打印**
```
String CPU_ABI = android.os.Build.CPU_ABI;
Log.d("pepe", "CPU_ABI = " + CPU_ABI);
```

Log:
```
D/pepe: CPU_ABI = armeabi-v7a
```

参考：

[如何查看Android设备的ABI - CSDN博客](https://blog.csdn.net/u014449046/article/details/78592526)