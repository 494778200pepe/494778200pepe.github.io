---
layout: post
title:  "java多线程“基础篇”03之Thread中的start()和run()的区别"
date:   2018-08-18 11:24:00 +0800
categories: Java
tags: Java多线程
author: pepe
description: 『 Thread中的start()和run()的区别 』
---

* `start()`: 它的作用是启动一个新线程，新线程会执行相应的run()方法。start()不能被重复调用。

* `run()`: run()就和普通的成员方法一样，可以被重复调用。单独调用run()的话，会在当前线程中执行run()，而并不会启动新线程！

## start() 和 run()相关源码(基于JDK1.7.0_40)

Thread.java中`start()`方法的源码如下：
```
public synchronized void start() {
    // 如果线程不是"就绪状态"，则抛出异常！
    if (threadStatus != 0)
        throw new IllegalThreadStateException();

    // 将线程添加到ThreadGroup中
    group.add(this);

    boolean started = false;
    try {
        // 通过start0()启动线程
        start0();
        // 设置started标记
        started = true;
    } finally {
        try {
            if (!started) {
                group.threadStartFailed(this);
            }
        } catch (Throwable ignore) {
        }
    }
}
```

说明：`start(`)实际上是通过本地方法`start0()`启动线程的。而`start0()`会新运行一个线程，新线程会调用`run()`方法。
```
private native void start0();
``` 

Thread.java中`run()`的代码如下：
```
public void run() {
    if (target != null) {
        target.run();
    }
}
```
说明：`target`是一个`Runnable对象`。run()就是直接调用Thread线程的Runnable成员的run()方法，并不会新建一个线程。




