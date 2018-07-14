---
layout: post
title:  "一、Handler、Message、MessageQueue、Looper调用过程"
date:   2018-07-14 08:46:00 +0800
categories: Android
tags: Handler
author: pepe
description: 『 Handler、Message、MessageQueue、Looper调用过程 』
---

### **调用过程**

* 1、`Handler`发送`Message`，`MessageQueue`将`Message`添加到队列中(先暂且这么理解)
* 2、`Looper`轮询从`MessageQueue`中取出一个个`Message`，并处理
* 3、`Message`调用`Handler`对自己进行处理。

> 在互相调用的过程中可以发现，最后返回了`queue.enqueueMessage(msg,uptimeMillis)`。这里的`enqueueMessage`方法的主要操作其实就是向`MessageQueue`中插入一条数据（注意：`MessageQueue`虽然翻译过来是消息队列，但是它的内部存储结构并不是真正的队列，而是采用单链表的数据结构来存储消息列表）。也就是说`Handler`发送消息的过程仅仅是向`MessageQueue`中插入了一条消息，`MessageQueue`的`next方法`就会返回这条消息给`Looper`，`Looper`收到消息后就开始处理了，最终消息由`Looper`交由`Handler`处理，即`Handler`的d`ispatchMessage`方法会被调用。这就是这四个类之间的调用逻辑。

### **相关对象**

* `Looper` 消息轮询器
* `MessageQueue` 消息暂存队列(单链表结构)
* `Message` 消息
* `Handler` 收发消息工具
* `ThreadLocal` (本地线程数据存储对象)

### **`Handler`发送`Message`**

发送消息：
```
public final boolean sendMessage (Message msg )
{
    return sendMessageDelayed(msg , 0);
}
 
public final boolean sendMessageDelayed (Message msg, long delayMillis )
{
    if (delayMillis < 0) {
        delayMillis = 0;
    }
    return sendMessageAtTime(msg , SystemClock.uptimeMillis() + delayMillis);
}
 
public boolean sendMessageAtTime (Message msg , long uptimeMillis) {
    MessageQueue queue = mQueue;
    if (queue == null) {
        RuntimeException e = new RuntimeException(
                this + " sendMessageAtTime() called with no mQueue");
        Log. w("Looper", e.getMessage(), e);
        return false ;
    }
    return enqueueMessage(queue , msg , uptimeMillis);
}
 
private boolean enqueueMessage(MessageQueue queue, Message msg , long uptimeMillis)                        { 
    msg.target = this;
    if (mAsynchronous ) {
        msg.setAsynchronous( true);
    }
    return queue.enqueueMessage(msg, uptimeMillis);
}
```


































