---
layout: post
title:  "java多线程“基础篇”02之Thread和Runnable"
date:   2018-08-18 11:09:00 +0800
categories: Java
tags: Java多线程
author: pepe
description: 『 Thread和Runnable 』
---

* Thread 和 Runnable 的相同点：都是“多线程的实现方式”。

* Thread 和 Runnable 的不同点：

    Thread 是类，而Runnable是接口；Thread本身是实现了Runnable接口的类。我们知道“一个类只能有一个父类，但是却能实现多个接口”，因此Runnable具有更好的扩展性。
此外，Runnable还可以用于“资源的共享”。即，多个线程都是基于某一个Runnable对象建立的，它们会共享Runnable对象上的资源。
通常，建议通过“Runnable”实现多线程！


参考：

[多线程-局部变量和成员变量 - CSDN博客](https://blog.csdn.net/qianyiyiding/article/details/77864118)



