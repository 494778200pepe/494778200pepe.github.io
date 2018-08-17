---
layout: post
title:  "SyncchronousQueue"
date:   2018-08-17 16:53:00 +0800
categories: Java
tags: Java基础
author: pepe
description: 『 SyncchronousQueue 』
---

> `SynchronousQueue`是这样一种阻塞队列，其中每个 put 必须等待一个 take，反之亦然。同步队列没有任何内部容量，甚至连一个队列的容量都没有。不能在同步队列上进行 peek，因为仅在试图要取得元素时，该元素才存在； 
        
除非另一个线程试图移除某个元素，否则也不能（使用任何方法）添加元素;也不能迭代队列，因为其中没有元素可用于迭代。队列的头是尝试添加到队列中的首个已排队线程元素； 如果没有已排队线程，则不添加元素并且头为 null。 
        
对于其他 Collection 方法（例如 contains），SynchronousQueue 作为一个空集合。此队列不允许 null 元素。
        
它非常适合于传递性设计，在这种设计中，在一个线程中运行的对象要将某些信息、事件或任务传递给在另一个线程中运行的对象，它就必须与该对象同步。 
        
对于正在等待的生产者和使用者线程而言，此类支持可选的公平排序策略。默认情况下不保证这种排序。但是，使用公平设置为 true 所构造的队列可保证线程以 FIFO 的顺序进行访问。 公平通常会降低吞吐量，但是可以减小可变性并避免得不到服务。 
       
### **注意**
       
* 注意1：它一种阻塞队列，其中每个 put 必须等待一个 take，反之亦然。同步队列没有任何内部容量，甚至连一个队列的容量都没有。 
* 注意2：它是线程安全的，是阻塞的。 
* 注意3: 不允许使用 null 元素。 
* 注意4：公平排序策略是指调用put的线程之间，或take的线程之间。公平排序策略可以查考ArrayBlockingQueue中的公平策略。 
* 注意5:SynchronousQueue的以下方法：

### **SynchronousQueue实现原理**

* 公平模式下的模型：双向队列，先进先出，随时匹配，从两头匹配。
* 非公平模式下的模型：一个栈，先进后出。

### **API说明**

* iterator() 永远返回空，因为里面没东西。 
* peek() 永远返回null。 
* put() 往queue放进去一个element以后就一直wait直到有其他thread进来把这个element取走。 
* offer() 往queue里放一个element后立即返回，如果碰巧这个element被另一个thread取走了，offer方法返回true，认为offer成功；否则返回false。 
* offer(2000, TimeUnit.SECONDS) 往queue里放一个element但是等待指定的时间后才返回，返回的逻辑和offer()方法一样。 
* take() 取出并且remove掉queue里的element（认为是在queue里的。。。），取不到东西他会一直等。 
* poll() 取出并且remove掉queue里的element（认为是在queue里的。。。），只有到碰巧另外一个线程正在往queue里offer数据或者put数据的时候，该方法才会取到东西。否则立即返回null。 
* poll(2000, TimeUnit.SECONDS) 等待指定的时间然后取出并且remove掉queue里的element,其实就是再等其他的thread来往里塞。 
* isEmpty()永远是true。 
* remainingCapacity() 永远是0。 
* remove()和removeAll() 永远是false。 

参考：

[java并发之SynchronousQueue实现原理 - CSDN博客](https://blog.csdn.net/yanyan19880509/article/details/52562039)

[SynchronousQueue的使用 - CSDN博客](https://blog.csdn.net/zmx729618/article/details/52980158)


[Java多线程总结之线程安全队列Queue - CSDN博客](
https://blog.csdn.net/madun/article/details/20313269)

















