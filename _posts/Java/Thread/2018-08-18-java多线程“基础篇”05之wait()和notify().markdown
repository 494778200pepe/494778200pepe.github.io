---
layout: post
title:  "java多线程“基础篇”05之wait()和notify()"
date:   2018-08-18 13:45:00 +0800
categories: Java
tags: Java多线程
author: pepe
description: 『 wait()和notify() 』
---

> `wait()的作用是让“当前线程”等待，而“当前线程”是指正在cpu上运行的线程！`

* `notify()`:唤醒在此对象监视器上等待的单个线程(随机的，与优先级无关)。

* `notifyAll()`:唤醒在此对象监视器上等待的所有线程。

* `wait()`:让当前线程处于“等待(阻塞)状态”，“直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法”，当前线程被唤醒(进入“就绪状态”)。

* `wait(long timeout)`:让当前线程处于“等待(阻塞)状态”，“直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量”，当前线程被唤醒(进入“就绪状态”)。当millis为0时，表示无限等待，与wait()效果一样。

* `wait(long timeout, int nanos)`:让当前线程处于“等待(阻塞)状态”，“直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量”，当前线程被唤醒(进入“就绪状态”)。nanos的单位为纳秒(1s = 1000ms = 1000 000ns)。

[java wait(long timeout, int nanos)，后面的nanos有什么用？](https://www.zhihu.com/question/41808470)



`线程正常结束后，会使以这个「线程对象」运行的wait()等待，退出等待状态！而如果在运行wait()之前，线程已经结束了，则这个wait就没有程序唤醒了。

[wait() 一定需要notify()唤醒吗？ - CSDN博客](https://blog.csdn.net/nmyangym/article/details/7850882)
