---
layout: post
title:  "java多线程“基础篇”04之synchronized关键字"
date:   2018-08-18 13:11:00 +0800
categories: Java
tags: Java多线程
author: pepe
description: 『 synchronized关键字 』
---

`在java中，每一个对象有且仅有一个同步锁。这也意味着，同步锁是依赖于对象而存在。`

> 当我们调用某对象的synchronized方法时，就获取了该对象的同步锁。例如，synchronized(obj)就获取了“obj这个对象”的同步锁。
不同线程对同步锁的访问是互斥的。也就是说，某时间点，对象的同步锁只能被一个线程获取到！通过同步锁，我们就能在多线程中，实现对“对象/方法”的互斥访问。 例如，现在有两个线程A和线程B，它们都会访问“对象obj的同步锁”。假设，在某一时刻，线程A获取到“obj的同步锁”并在执行一些操作；而此时，线程B也企图获取“obj的同步锁” —— 线程B会获取失败，它必须等待，直到线程A释放了“该对象的同步锁”之后线程B才能获取到“obj的同步锁”从而才可以运行。

### **synchronized基本规则**

* 第一条: 当一个线程访问“某对象”的“synchronized方法”或者“synchronized代码块”时，其他线程对“该对象”的该“synchronized方法”或者“synchronized代码块”的访问将被阻塞。


* 第二条: 当一个线程访问“某对象”的“synchronized方法”或者“synchronized代码块”时，其他线程仍然可以访问“该对象”的非同步代码块。

* 第三条: 当一个线程访问“某对象”的“synchronized方法”或者“synchronized代码块”时，其他线程对“该对象”的其他的“synchronized方法”或者“synchronized代码块”的访问将被阻塞。

 
### **实例锁和全局锁**

* 实例锁 -- 锁在某一个实例对象上。如果该类是单例，那么该锁也具有全局锁的概念。

               实例锁对应的就是synchronized关键字。
               
* 全局锁 -- 该锁针对的是类，无论实例多少个对象，那么线程都共享该锁。

               全局锁对应的就是static synchronized（或者是锁在该类的class或者classloader对象上）。

关于“实例锁”和“全局锁”有一个很形象的例子：
```
pulbic class Something {
    public synchronized void isSyncA(){}
    public synchronized void isSyncB(){}
    public static synchronized void cSyncA(){}
    public static synchronized void cSyncB(){}
}
```

假设，Something有两个实例x和y。分析下面4组表达式获取的锁的情况。

* (01) `x.isSyncA()与x.isSyncB() `:不能被同时访问

* (02) `x.isSyncA()与y.isSyncA()`:可以被同时访问

* (03) `x.cSyncA()与y.cSyncB()`:不能被同时访问

* (04) `x.isSyncA()与Something.cSyncA()`:可以被同时访问



