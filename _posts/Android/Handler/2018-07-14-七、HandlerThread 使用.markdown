---
layout: post
title:  "七、HandlerThread 使用"
date:   2018-07-14 11:10:00 +0800
categories: Android
tags: Handler
author: pepe
description: 『 HandlerThread 使用 』
---

### `Handler`学习系列

* [一、Handler、Message、MessageQueue、Looper调用过程](
https://494778200pepe.github.io/android/2018/07/14/%E4%B8%80-Handler-Message-MessageQueue-Looper%E8%B0%83%E7%94%A8%E8%BF%87%E7%A8%8B.html)
* [二、Handler 线程切换的原理](https://494778200pepe.github.io/android/2018/07/14/%E4%BA%8C-Handler-%E7%BA%BF%E7%A8%8B%E5%88%87%E6%8D%A2%E7%9A%84%E5%8E%9F%E7%90%86.html)
* [三、什么是ThreadLocal？](https://494778200pepe.github.io/android/2018/07/14/%E4%B8%89-%E4%BB%80%E4%B9%88%E6%98%AFThreadLocal.html)
* [四、Handler使用中的内存泄漏](https://494778200pepe.github.io/android/2018/07/14/%E5%9B%9B-Handler%E4%BD%BF%E7%94%A8%E4%B8%AD%E7%9A%84%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F.html)
* [五、Handler的消息驱动模型](https://494778200pepe.github.io/android/2018/07/14/%E4%BA%94-Handler%E7%9A%84%E6%B6%88%E6%81%AF%E9%A9%B1%E5%8A%A8%E6%A8%A1%E5%9E%8B.html)
* [六、单向链表](https://494778200pepe.github.io/android/2018/07/14/%E5%85%AD-%E5%8D%95%E5%90%91%E9%93%BE%E8%A1%A8.html)
* [七、HandlerThread 使用](https://494778200pepe.github.io/android/2018/07/14/%E4%B8%83-HandlerThread-%E4%BD%BF%E7%94%A8.html)


> `HandlerThread`简单来说，就是在用`Handler`封装了一个`Thread`，使`子线程Thread`也支持使用`Handler`做线程切换和`Looper`的消息处理方式。

### **HandlerThread源码分析**
```
public class HandlerThread extends Thread {
    Looper mLooper;

    @Override
    public void run() {
        mTid = Process.myTid();
        Looper.prepare();
        synchronized (this) {
            mLooper = Looper.myLooper();//创建Looper
            notifyAll();
        }
        Process.setThreadPriority(mPriority);
        onLooperPrepared();
        Looper.loop();//开启消息轮训
        mTid = -1;
    }
    public Looper getLooper() {  return mLooper;}
    public boolean quit() {    looper.quit();    }
    public boolean quitSafely() {   looper.quitSafely(); }
}

```
### **如果没有Handler和HandlerThread以前**

我们先来看一个没有这种机制以前是怎么处理的。

下面是伪代码：
```
Thread mTh1 =new Thread(()->{
        ·····
    //我做完了，现在想通知mainTh怎么办？？？
   });

Thread mainTh=new Thread(()->{
     //做一些事情
        ·····
   //做完了调用
    mTh1.start();
   });

mainTh.start(); //开始做事
```

这段代码很简单，我们有两个线程，`mainTh`做完某些事情以后将启动子线程`mTh1`来执行。这些都没有问题，但当`mTh1`完成任务以后，它想再次回到主线程中告知`mainTh`怎么办？是不是思路瞬间短路了。
为什么？因为线程总是顺序执行的，而且是并行顺序执行，一旦执行就没有回路。

### **当HandlerThread出现以后**

还是上面这个功能，我们看伪代码。
```
protected void onCreate(Bundle savedInstanceState) {
    mHThread = new HandlerThread( "mHThread") ;
    mHThread .start();
    mMainHandler = new Handler(){
        @Override
        public void handleMessage(Message msg) {
            //这里收到的消息将会在主线程中执行

        }
    };

    mHandler = new Handler(mHThread.getLooper()){
        @Override
        public void handleMessage(Message msg) {
            //这里收到的消息将会在mHThread子线程中处理
                        ·····
            //我做完了，现在将消息通知回主线程
             mMainHandler.sendEmptyMessage(2) ;
        }
    };
      
    //主线程做一些事情
        ········
    //做完了调用mHandler通知子线程mHThread 
    mHandler.sendEmptyMessage(1) ;
}

```
是不是方便了很多。

参考：

[HandlerThread线程间通信 源码解析 - 简书](https://www.jianshu.com/p/69c826c8a87d)