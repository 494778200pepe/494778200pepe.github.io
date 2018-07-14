---
layout: post
title:  "一、Handler、Message、MessageQueue、Looper调用过程"
date:   2018-07-14 08:46:00 +0800
categories: Android
tags: Handler
author: pepe
description: 『 Handler、Message、MessageQueue、Looper调用过程 』
---

### 相关对象

* `Looper` 消息轮训器
* `MessageQueue` 消息暂存队列(单链表结构)
* `Message` 消息
* `Handler` 收发消息工具
* `ThreadLocal` (本地线程数据存储对象)