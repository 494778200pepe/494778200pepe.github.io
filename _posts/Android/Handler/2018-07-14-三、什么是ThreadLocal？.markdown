---
layout: post
title:  "三、什么是ThreadLocal？"
date:   2018-07-14 09:50:00 +0800
categories: Android
tags: Handler
author: pepe
description: 『 什么是ThreadLocal？ 』
---

在前面的分析中，涉及到`ThreadLocal`的地方有：
#### **Looper**
```
static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();

    /**
     * Return the Looper object associated with the current thread.  Returns
     * null if the calling thread is not associated with a Looper.
     */
    public static Looper myLooper() {
        // 从mThreadLocak中取出Looper
        return sThreadLocal .get();
    }
```

#### **Looper.prepare()**
```
    private static void More ...prepare(boolean quitAllowed) {
        if (sThreadLocal.get() != null) {
            throw new RuntimeException("Only one Looper may be created per thread");
        }
        // 将Looper存储在sThreadLocal中
        sThreadLocal.set(new Looper(quitAllowed));
    }
```
#### **Looper.loop()**
```
    public static void loop() {
        final Looper me = myLooper ();
        
        // 省略...
    }  
```

### **Handler()**
```
    // Handler的构造方法
    public Handler(Callback callback, boolean async) {
        if (FIND_POTENTIAL_LEAKS) {
            final Class<? extends Handler> klass = getClass();
            if ((klass.isAnonymousClass() || klass.isMemberClass() || klass.isLocalClass()) &&
                    (klass.getModifiers() & Modifier.STATIC) == 0) {
                Log.w(TAG, "The following Handler class should be static or leaks might occur: " +
                        klass.getCanonicalName());
            }
        }
        
        mLooper = Looper.myLooper();
        if (mLooper == null) {
            throw new RuntimeException(
                    "Can't create handler inside thread that has not called Looper.prepare()");
        }
        mQueue = mLooper.mQueue;
        mCallback = callback;
        mAsynchronous = async;
    }
    
    

```

综上，使用`ThreadLocal`的地方有：

* 1、`Looper`持有`ThreadLocal`对象。
* 2、`Looper`在`prepare()`时，对`ThreadLocal`进行赋值初始化，将`Looper`对象保存。
* 3、`Looper.loop()`和`Handler`通过静态方法`Looper.myLooper()`调用从`ThreadLocal`中得到保存好的`Looper`对象。
* 4、所以，`ThreadLocal`是用来 中转 `Looper`对象的。



[ThreadLocal源码解析 - CSDN博客](https://blog.csdn.net/qq_17250009/article/details/50000753)

[Android的消息机制之ThreadLocal的工作原理 - CSDN博客](https://blog.csdn.net/singwhatiwanna/article/details/48350919)









































