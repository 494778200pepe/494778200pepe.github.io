---
layout: post
title:  "RxJava 例子 - doOnSubscribe"
date:   2020-11-28 11:33:00 +0800
categories: Android
tags: RxJava
author: pepe
description: 『 RxJava 例子 - doOnSubscribe 』
---

```
 Observable.from(items)
                .map(new Func1<Integer, String>() {
                    @Override
                    public String call(Integer integer) {
                        Log.d(TAG, "map 所在的线程，子线程 = " + Thread.currentThread().getName());
                        return integer + "";
                    }
                })
                .subscribeOn(Schedulers.io()) // subscribeOn the I/O thread
                .doOnSubscribe(new Action0() {
                    @Override
                    public void call() {
                        Log.d(TAG, "第一个 doOnSubscribe，子线程");
                        Log.d(TAG, "第二个 doOnSubscribe，线程 = " + Thread.currentThread().getName());
                    }
                })
                .subscribeOn(Schedulers.io()) // subscribeOn the I/O thread
                .doOnSubscribe(new Action0() {
                    @Override
                    public void call() {
                        Log.d(TAG, "第一个 doOnSubscribe，主线程");
                        Log.d(TAG, "第一个 doOnSubscribe，线程 = " + Thread.currentThread().getName());
                    }
                })
                .subscribeOn(AndroidSchedulers.mainThread()) // subscribeOn the I/O thread
                .observeOn(AndroidSchedulers.mainThread()) // observeOn the UI Thread
                .subscribe(new Action1<String>() {
                    @Override
                    public void call(String str) {
                        log(str);
                    }
                });
```