---
layout: post
title:  "三、什么是ThreadLocal？"
date:   2018-07-14 09:50:00 +0800
categories: Android
tags: Handler
author: pepe
description: 『 什么是ThreadLocal？ 』
---

> 它是一个线程内部的数据存储类，通过它可以再指定的线程中存储数据，数据存储以后，只有在指定线程中可以获取到存储的数据，对于其它线程来说则无法获取到数据。对于`Handler`来说，它需要获取当前线程的`Looper`，很显然`Looper`的作用域就是线程并且不同线程具有不同的`Looper`，这个时候通过`ThreadLocal`就可以轻松实现`Looper`在线程中的存取。

在前面的分析中，涉及到`ThreadLocal`的地方有：
#### **Looper**
```
// 一个静态常量
static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();

    /**
     * Return the Looper object associated with the current thread.  Returns
     * null if the calling thread is not associated with a Looper.
     */
    public static Looper myLooper() {
        // 从mThreadLocal中取出Looper
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

### **`ThreadLocal`工作原理**

#### `Looper`的获取
在`Looper`中：
```
    public static Looper myLooper() {
        // 从mThreadLocal中取出Looper
        return sThreadLocal .get();
    }
```
在`ThreadLocal`中：
```
    public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null)
                return (T)e.value;
        }
        return setInitialValue();
    }
    
    ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
    }
    
    private T setInitialValue() {
        T value = initialValue();
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
        return value;
    }
    
    protected T initialValue() {
        return null;
    }
    
    void createMap(Thread t, T firstValue) {
        t.threadLocals = new ThreadLocalMap(this, firstValue);
    }
```
简要分析下：

* 1、拿到当前线程`t`
* 2、从`t`获取一个`ThreadLocalMap`
* 3、以`ThreadLocal`(`Looper`的成员变量`sThreadLocal`)为`key`，获取`value`(也就是`Looper`对象了)
* 4、如果`ThreadLocalMap`为空，那么初始化并返回一个空值

#### `Looper`的保存
```
    // ThreadLocal 中
    public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }
```
可以看到，是保存在`ThreadLocalMap`中的。

#### `ThreadLocalMap`
在`ThreadLocal`中：
```
    static class ThreadLocalMap {
```
`ThreadLocalMap`原来是`ThreadLocal`的一个静态的内部类。

在`Thread`中：
```
    /* ThreadLocal values pertaining to this thread. This map is maintained
     * by the ThreadLocal class. */
    ThreadLocal.ThreadLocalMap threadLocals = null;
```
每个`Thread`都持有一个`ThreadLocal.ThreadLocalMap`对象。

> 结论：每个线程都有一个`ThreadLocal.ThreadLocalMap`对象，同时`ThreadLocalMap`又以`Looper`的`sThreadLocal`为`key`，保存了`Thread`保存的`value`。

#### `Looper`与`Thread`
```
    // Looper 的构造方法
    private static void prepare(boolean quitAllowed) {
        if (sThreadLocal.get() != null) {
            throw new RuntimeException("Only one Looper may be created per thread");
        }
        sThreadLocal.set(new Looper(quitAllowed));
    }
    
    // ThreadLocal 中
    public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }
    
    public static Looper myLooper() {
        // 从mThreadLocal中取出Looper
        return sThreadLocal .get();
    }
```
从上面可以看到每个`Thread`，只能调用一次`Looper.prepare()`。`Thread`持有的`Looper`对象保存在所有`Thread`持有的`ThreadLocalMap`中。

看到这里，相信你能明白`ThreadLocal`是干啥的了吧~

> `Looper`在`prepare()`时，构建`Thread`的`ThreadLocalMap`，同时以`sThreadLocal`为`key`，保存`Looper`对象。在各自线程通过`Thread`对象，拿到各自的`ThreadLocalMap`。由于`ThreadLocal`是一个静态常量，被`Thread`持有(因为<font size="5" color="#dd0000">Looper的作用域就是线程并且不同线程具有不同的Looper</font><br /> )，所以可以直接拿到，当作`key`去获取`Looper`对象。

参考：

[ThreadLocal源码解析 - CSDN博客](https://blog.csdn.net/qq_17250009/article/details/50000753)

[Android的消息机制之ThreadLocal的工作原理 - CSDN博客](https://blog.csdn.net/singwhatiwanna/article/details/48350919)









































