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
* 2、`Looper`轮询从`MessageQueue`中取出一个个`Message`，如果没有则等待
* 3、`Message`调用`Handler`对自己进行处理

> 在互相调用的过程中可以发现，最后返回了`queue.enqueueMessage(msg,uptimeMillis)`。这里的`enqueueMessage`方法的主要操作其实就是向`MessageQueue`中插入一条数据（注意：`MessageQueue`虽然翻译过来是消息队列，但是它的内部存储结构并不是真正的队列，而是采用单链表的数据结构来存储消息列表）。也就是说`Handler`发送消息的过程仅仅是向`MessageQueue`中插入了一条消息，`MessageQueue`的`next方法`就会返回这条消息给`Looper`，`Looper`收到消息后就开始处理了，最终消息由`Looper`交由`Handler`处理，即`Handler`的d`ispatchMessage`方法会被调用。这就是这四个类之间的调用逻辑。

### **相关对象**

* `Looper` 消息轮询器
* `MessageQueue` 消息暂存队列(单链表结构)
* `Message` 消息
* `Handler` 收发消息工具
* `ThreadLocal` (本地线程数据存储对象)

### **1、`Handler`发送`Message`**

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
可以看到，所有发送消息到最后都是`MessageQueue`将`Message`添加到队列中。

### **2、`Looper`轮询取出`Message`**

#### 第一步，先来了解一下`Looper`是如何取出`Message`**的：

前面说`MessageQueue`是一个队列，这个表述其实是错误的。`MessageQueue`其实是一个存储`Message`的一个单链表，重要的是两个方法，`enqueueMessage`和`next`。`enqueueMessage`刚已经说过了，其主要操作是向`MessageQueue`单链表中插入数据。下面主要看一下`next方法`。
```
    Message next () {
        int pendingIdleHandlerCount = -1; // -1 only during first iteration
        int nextPollTimeoutMillis = 0;
        for (;;) {
            if (nextPollTimeoutMillis != 0) {
                Binder. flushPendingCommands();
            }
 
            // We can assume mPtr != 0 because the loop is obviously still running.
            // The looper will not call this method after the loop quits.
            nativePollOnce(mPtr, nextPollTimeoutMillis);
 
            synchronized (this ) {
                // Try to retrieve the next message.  Return if found.
                final long now = SystemClock.uptimeMillis();
                Message prevMsg = null;
                Message msg = mMessages;
                if (msg != null && msg.target == null) {
                    // Stalled by a barrier.  Find the next asynchronous message in                                    -                   //the queue.
                    do {
                        prevMsg = msg;
                        msg = msg.next;
                    } while (msg != null && !msg.isAsynchronous());
                }
                if (msg != null) {
                    if (now < msg .when) {
                        // Next message is not ready.  Set a timeout to wake up when    -                       //it is ready.
                        nextPollTimeoutMillis = (int) Math.min( msg.when - now,                                                                 -                       Integer.MAX_VALUE);
                    } else {
                        // Got a message.
                        mBlocked = false;
                        if (prevMsg != null) {
                            prevMsg.next = msg.next;
                        } else {
                            mMessages = msg .next;
                        }
                        msg.next = null;
                        if (false ) Log.v("MessageQueue", "Returning message: " +msg);
                        msg.markInUse();
                        return msg ;
                    }
                } else {
                    // No more messages.
                    nextPollTimeoutMillis = -1;
                }
                              ...
    }
```
可以看到是`next`是一个无线循环的方法，唯一跳出循环的条件是取出`MessageQueue`中的`Message`,然后`return msg`。如果`MessageQueue `中没有消息，那么`next方法`将一直阻塞在这里。当有新消息到来时，`next方法`会返回这条消息并将其从`MessageQueue`中删除。

既然`Message`是从`next方法`中取走的，那么是谁来调用`next()`的呢？答案是：`Looper.loop();`
```
    public static void loop() {
        final Looper me = myLooper ();
        if (me == null) {
            throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
        }
        final MessageQueue queue = me .mQueue ;
 
        // Make sure the identity of this thread is that of the local process,
        // and keep track of what that identity token actually is.
        Binder. clearCallingIdentity();
        final long ident = Binder.clearCallingIdentity();
 
        for (;;) {
            Message msg = queue.next(); // might block
            if (msg == null) {
                // No message indicates that the message queue is quitting.
                return;
            }
 
            // This must be in a local variable, in case a UI event sets the logger
            Printer logging = me. mLogging;
            if (logging != null) {
                logging.println( ">>>>> Dispatching to " + msg.target + " " +
                        msg.callback + ": " + msg.what );
            }
 
            msg.target.dispatchMessage( msg);
 
            if (logging != null) {
                logging.println( "<<<<< Finished to " + msg.target + " "+msg. callback);
            }
 
            // Make sure that during the course of dispatching the
            // identity of the thread wasn't corrupted.
            final long newIdent = Binder.clearCallingIdentity();
            if (ident != newIdent ) {
                Log. wtf(TAG, "Thread identity changed from 0x"
                        + Long. toHexString(ident) + " to 0x"
                        + Long.toHexString(newIdent) + " while dispatching to "
                        + msg.target.getClass().getName() + " "
                        + msg.callback + " what=" + msg.what );
            }
 
            msg.recycle();
        }
    }
 
    /**
     * Return the Looper object associated with the current thread.  Returns
     * null if the calling thread is not associated with a Looper.
     */
    public static Looper myLooper() {
        // 从mThreadLocak中取出Looper
        return sThreadLocal .get();
    }
```
可以看到，`loop()`里面也是一个死循环`for (;;)`，不断从`Message msg = queue.next()`中读取`Message`。同时，如果`MessageQueue`中没有消息，`queue.next()`阻塞，那么`loop()`也将阻塞。只有执行`Loop.quit/quitSafely`才会跳出循环。如果取到了`Message`，那么执行`msg.target.dispatchMessage( msg);`。

#### 第二步，`Looper`是什么时候开始轮询`loop()`的？
追根溯源，`Looper.loop()`方法是由`ActivityThread`的`Main()`方法去调用的，如下所示：
```
    public static void More ...main(String[] args) {
        SamplingProfilerIntegration.start();
 
        // CloseGuard defaults to true and can be quite spammy.  We
        // disable it here, but selectively enable it later (via
        // StrictMode) on debug builds, but using DropBox, not logs.
        CloseGuard.setEnabled(false);
 
        Environment.initForCurrentUser();
 
        // Set the reporter for event logging in libcore
        EventLogger.setReporter(new EventLoggingReporter());
 
        Security.addProvider(new AndroidKeyStoreProvider());
 
        // Make sure TrustedCertificateStore looks in the right place for CA certificates
        final File configDir = Environment.getUserConfigDirectory(UserHandle.myUserId());
        TrustedCertificateStore.setDefaultUserDirectory(configDir);
 
        Process.setArgV0("<pre-initialized>");
 
        Looper.prepareMainLooper();
 
        ActivityThread thread = new ActivityThread();
        thread.attach(false);
 
        if (sMainThreadHandler == null) {
            sMainThreadHandler = thread.getHandler();
        }
 
        if (false) {
            Looper.myLooper().setMessageLogging(new
                    LogPrinter(Log.DEBUG, "ActivityThread"));
        }
 
       Looper.loop();
 
       throw new RuntimeException("Main thread loop unexpectedly exited");
    }
```
可以看到，在`Looper.loop();`之前，先执行了`Looper.prepareMainLooper();`。
```
    public static void More ...prepareMainLooper() {
        prepare(false);
        synchronized (Looper.class) {
            if (sMainLooper != null) {
                throw new IllegalStateException("The main Looper has already been prepared.");
            }
            sMainLooper = myLooper();
        }
    }
     
    private static void More ...prepare(boolean quitAllowed) {
        if (sThreadLocal.get() != null) {
            throw new RuntimeException("Only one Looper may be created per thread");
        }
        // 将Looper存储在sThreadLocal中
        sThreadLocal.set(new Looper(quitAllowed));
    }
```
### **3、`Message`调用`Handler`对自己进行处理**
在`Looper.loop()`中可以看到，`Message`的处理调用的是：
```
msg.target.dispatchMessage( msg);
```
这个`target`是什么呢？又是什么时候赋值的呢？回到`sendMessage()`
```
private boolean enqueueMessage(MessageQueue queue, Message msg , long uptimeMillis)                        { 
    msg.target = this;
    if (mAsynchronous ) {
        msg.setAsynchronous( true);
    }
    return queue.enqueueMessage(msg, uptimeMillis);
}
```
原来是`enqueueMessage()`时，就将`Handler`赋值给`Message`了。同时，在`MessageQueue`的`next()`中又对`target`进行了判空：
```
// 如果msg.target == null,则不取出该消息
if (msg != null && msg.target == null) {
    // Stalled by a barrier.  Find the next asynchronous message in                                    -                   //the queue.
    do {
        prevMsg = msg;
        msg = msg.next;
    } while (msg != null && !msg.isAsynchronous());
}
```

再来看看`Handler`对`Message`的处理方式：
```
    public void dispatchMessage(Message msg) {
        if (msg .callback != null) {
            handleCallback(msg);
        } else {
            if (mCallback != null) {
                if (mCallback .handleMessage(msg)) {
                    return;
                }
            }
            handleMessage( msg);
        }
    }
 
    public interface Callback {
        public boolean handleMessage(Message msg);
    }
 
    private static void handleCallback(Message message) {
        message.callback.run();
    }
```

* 1、`Message`如果有设置`callback`，那么执行该回调
* 2、`Message`如果没有设置`callback`那么执行`Handler`的`callback`


![Handler]({{ site.baseurl }}/assets/images/android/Handler/handler1.png)
再看这张图应该很清晰了吧~


参考：

[Handler、Message、MessageQueue、Looper调用过程源码浅析 - CSDN博客](https://blog.csdn.net/qq_17250009/article/details/50017237)

[看完这篇文章，你就了解了Android Handler的一切  20130816 - CSDN博客](http://blog.csdn.net/u011733020/article/details/49589863)

[Handler消息源码流程分析（含手写笔记） - 简书](https://www.jianshu.com/p/6f25729ef62a)




















