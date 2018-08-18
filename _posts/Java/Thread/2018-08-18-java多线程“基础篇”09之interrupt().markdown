---
layout: post
title:  "java多线程“基础篇”09之interrupt()"
date:   2018-08-18 16:05:00 +0800
categories: Java
tags: Java多线程
author: pepe
description: 『 interrupt() 』
---


```
Interrupts this thread.
Unless the current thread is interrupting itself, which is always permitted, the checkAccess method of this thread is invoked, which may cause a SecurityException to be thrown.

If this thread is blocked in an invocation of the wait(), wait(long), or wait(long, int) methods of the Object class, or of the join(), join(long), join(long, int), sleep(long), or sleep(long, int), methods of this class, then its interrupt status will be cleared and it will receive an InterruptedException.

If this thread is blocked in an I/O operation upon an interruptible channel then the channel will be closed, the thread's interrupt status will be set, and the thread will receive a ClosedByInterruptException.

If this thread is blocked in a Selector then the thread's interrupt status will be set and it will return immediately from the selection operation, possibly with a non-zero value, just as if the selector's wakeup method were invoked.

If none of the previous conditions hold then this thread's interrupt status will be set.

Interrupting a thread that is not alive need not have any effect.
```

* 当线程由于被调用了sleep(), wait(), join()等方法而进入阻塞状态,若此时调用线程的interrupt()将线程的中断标记设为true。由于处于阻塞状态，中断标记会被清除，同时产生一个InterruptedException异常。

* 当线程由于IO而而进入阻塞状态，若此时调用线程的interrupt()，则将线程的中断标记设为true，同时产生一个ClosedByInterruptException异常。

* 当线程被阻塞在一个Selector选择器中，若此时调用线程的interrupt()，则将线程的中断标记设为true，同时立即返回一个非零值，就像调用了选择器的唤醒方法一样。

* 当线程正常运行(非阻塞)时，若此时调用线程的interrupt()，interrupt()并不会终止处于“运行状态”的线程！它会将线程的中断标记设为true。通过`isInterrupted()`可以拿到该状态。

### **interrupted()和isInterrupted()的区别**

* interrupted() 和 isInterrupted()都能够用于检测对象的“中断标记”。

* 区别是，interrupted()除了返回中断标记之外，它还会清除中断标记(即将中断标记设为false)；而isInterrupted()仅仅返回中断标记。