---
layout: post
title:  "IntentService"
date:   2018-07-16 13:44:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 IntentService 』
---

> `IntentService`继承自`Service`本质上就是一个服务。但它内部拥有一个完整的`HandlerThread`。可以这样说`IntentService = Service + HandlerThread`。

源码：
```
public abstract class IntentService extends Service {
    private volatile Looper mServiceLooper;
    private volatile ServiceHandler mServiceHandler;
    private String mName;
    private boolean mRedelivery;

    private final class ServiceHandler extends Handler {
        public ServiceHandler(Looper looper) {
            super(looper);
        }
        @Override
        public void handleMessage(Message msg) {
            onHandleIntent((Intent)msg.obj);
            stopSelf(msg.arg1);
        }
    }

    @Override
    public void onCreate() {
        super.onCreate();
        HandlerThread thread = new HandlerThread("IntentService[" + mName + "]");
        thread.start();
        mServiceLooper = thread.getLooper();
        mServiceHandler = new ServiceHandler(mServiceLooper);
    }

    @Override
    public void onStart(@Nullable Intent intent, int startId) {
        Message msg = mServiceHandler.obtainMessage();
        msg.arg1 = startId;
        msg.obj = intent;
        mServiceHandler.sendMessage(msg);
    }

    @Override
    public void onDestroy() {
        mServiceLooper.quit();
    }
    protected abstract void onHandleIntent(@Nullable Intent intent);
}
```















参考：

[IntentService源码解析 - 简书](https://www.jianshu.com/p/83d9a3e09f0a)






