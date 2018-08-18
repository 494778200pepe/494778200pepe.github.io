---
layout: post
title:  "BlockingQueue"
date:   2018-08-17 16:53:00 +0800
categories: Java
tags: Java基础
author: pepe
description: 『 BlockingQueue 』
---

### **阻塞队列分类**

* `ArrayBlockingQueue`：一个由数组结构组成的有界阻塞队列。 
* `LinkedBlockingQueue`：一个由链表结构组成的有界阻塞队列。 
* `PriorityBlockingQueue`：一个支持优先级排序的无界阻塞队列。 
* `DelayQueue`：一个使用优先级队列实现的无界阻塞队列。 
* `SynchronousQueue`：一个不存储元素的阻塞队列。 
* `LinkedTransferQueue`：一个由链表结构组成的无界阻塞队列。 
* `LinkedBlockingDeque`：一个由链表结构组成的双向阻塞队列。

![BlockingQueue]({{ site.baseurl }}/assets/images/java/BlockingQueue.png)

## BlockingQueue的核心方法

### **放入数据**
　　
* `offer(anObject)`:表示如果可能的话,将anObject加到BlockingQueue里,即如果BlockingQueue可以容纳,则返回true,否则返回false.（本方法不阻塞当前执行方法的线程）
　　
* `offer(E o, long timeout, TimeUnit unit)`:可以设定等待的时间，如果在指定的时间内，还不能往队列中加入BlockingQueue，则返回失败。
　　
* `put(anObject)`:把anObject加到BlockingQueue里,如果BlockQueue没有空间,则调用此方法的线程被阻断直到BlockingQueue里面有空间再继续.

### **获取数据**
　　
* `poll(time)`:取走BlockingQueue里排在首位的对象,若不能立即取出,则可以等time参数规定的时间,取不到时返回null;
　　
* `poll(long timeout, TimeUnit unit)`：从BlockingQueue取出一个队首的对象，如果在指定时间内，队列一旦有数据可取，则立即返回队列中的数据。否则知道时间超时还没有数据可取，返回失败。
　　
* `take()`:取走BlockingQueue里排在首位的对象,若BlockingQueue为空,阻断进入等待状态直到BlockingQueue有新的数据被加入; 
　　
* `drainTo()`:一次性从BlockingQueue获取所有可用的数据对象（还可以指定获取数据的个数）， 通过该方法，可以提升获取数据效率；不需要多次分批加锁或释放锁。

参考：

[Java多线程-工具篇-BlockingQueue - jack.yujun - 博客园](http://www.cnblogs.com/jackyuj/archive/2010/11/24/1886553.html)

[Java多线程总结之聊一聊Queue - I'm Sure - ITeye博客](http://hellosure.iteye.com/blog/1126541s)


