---
layout: post
title:  "MyThreadPool"
date:   2018-05-12 11:29:00 +0800
categories: Android
tags: Util
author: pepe
description: 『 线程工具类 』
---

```
public class MyThreadPool {

    private static MyThreadPool instance;

    private MyThreadPool() {
    }

    public synchronized static MyThreadPool getInstance() {
        if (instance == null) {
            instance = new MyThreadPool();
        }
        return instance;
    }

    /**
     * 线程池
     */
    private ScheduledThreadPoolExecutor mThreadPool;

    /**
     * 执行任务
     * @param task 任务
     * @return 任务
     */
    public void executeTask(TimerTask task) {
        getScheduledThreadPoolExecutor().execute(task);
    }

    /**
     * 执行任务
     * @param task         任务
     * @param initialDelay 执行延迟
     * @return 任务
     */
    public ScheduledFuture executeTask(TimerTask task, int initialDelay) {
        return getScheduledThreadPoolExecutor().schedule(task, initialDelay, TimeUnit.MILLISECONDS);
    }

    /**
     * schedule:下一次的执行时间点=上一次程序  完成  执行的时间点+间隔时间
     * scheduleAtFixedRate:下一次的执行时间点=上一次程序  开始  执行的时间点+间隔时间
     * @param task         任务
     * @param initialDelay 执行延迟
     * @param period       执行周期
     * @return 任务
     */
    public ScheduledFuture executeTask(TimerTask task, long initialDelay,
                                       long period) {
        return getScheduledThreadPoolExecutor().scheduleAtFixedRate(task, initialDelay, period, TimeUnit.MILLISECONDS);
    }

    /**
     * 执行任务
     * @param runnable 任务
     * @return 任务
     */
    public void executeTask(Runnable runnable) {
        getScheduledThreadPoolExecutor().execute(runnable);
    }

    /**
     * 执行任务
     * @param runnable     任务
     * @param initialDelay 执行延迟
     * @return 任务
     */
    public void executeTask(Runnable runnable, long initialDelay) {
        getScheduledThreadPoolExecutor().schedule(runnable, initialDelay, TimeUnit.MILLISECONDS);
    }

    /**
     * 执行任务
     * @param runnable     任务
     * @param initialDelay 执行延迟
     * @param period       执行周期
     * @return 任务
     */
    public void executeTask(Runnable runnable, long initialDelay, long period) {
        getScheduledThreadPoolExecutor().scheduleAtFixedRate(runnable, initialDelay, period, TimeUnit.MILLISECONDS);
    }

    /**
     * 获取线程池,线程池数量为cpu核心数+1
     */
    private ScheduledThreadPoolExecutor getScheduledThreadPoolExecutor() {
        if (mThreadPool == null) {
            mThreadPool = new ScheduledThreadPoolExecutor(getNumCores() + 1);
        }
        return mThreadPool;
    }

    /**
     * 获取cpu核心数
     */
    private int getNumCores() {
        class CpuFilter implements FileFilter {
            @Override
            public boolean accept(File pathname) {
                return Pattern.matches("cpu[0-9]", pathname.getName());
            }
        }
        try {
            File dir = new File("/sys/devices/system/cpu/");
            File[] files = dir.listFiles(new CpuFilter());
            if (files.length > 5) {
                return 2;
            } else {
                return files.length;
            }
        } catch (Exception e) {
            Logger.d("error : " + e);
            return 2;
        }
    }
}
```






