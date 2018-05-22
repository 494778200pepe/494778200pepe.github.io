---
layout: post
title:  "MyThreadPool"
date:   2018-05-12 09:29:00 +0800
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
     * schedule(callable/runnable, delay,unit):第一个参数任务，第二个参数表示执行任务前等待的时间，第三参数表示时间单位。
     * @param task         任务
     * @param initialDelay 执行延迟
     * @return 任务
     */
    public ScheduledFuture executeTask(TimerTask task, int initialDelay) {
        return getScheduledThreadPoolExecutor().schedule(task, initialDelay, TimeUnit.MILLISECONDS);
    }

    /**
     * 第一个参数表示周期线执行的任务，第二个参数表示第一次执行前的延迟时间，第三个参数表示任务启动间隔时间，第四个参数表示时间单位。虽然任务类型是Runnable但该方法有返回值ScheduledFuture。可以通过该对象获取线程信息。
     * @param task         任务
     * @param initialDelay 执行延迟
     * @param period       执行周期
     * @return 任务
     */
    public ScheduledFuture executeAtFixedTask(TimerTask task, long initialDelay,
                                              long period) {
        return getScheduledThreadPoolExecutor().scheduleAtFixedRate(task, initialDelay, period, TimeUnit.MILLISECONDS);
    }

    /**
     * 与scheduleAtFixedRate方法类似，不同的是第三个参数表示前一次结束的时间和下一次任务启动的间隔时间。
     * @param task         任务
     * @param initialDelay 执行延迟
     * @param period       前一次结束的时间和下一次任务启动的间隔时间
     * @return
     */
    public ScheduledFuture executeWithFixedTask(TimerTask task, long initialDelay,
                                                long period) {
        return getScheduledThreadPoolExecutor().scheduleWithFixedDelay(task, initialDelay, period, TimeUnit.MILLISECONDS);
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
     * 第一个参数表示周期线执行的任务，第二个参数表示第一次执行前的延迟时间，第三个参数表示任务启动间隔时间，第四个参数表示时间单位。虽然任务类型是Runnable但该方法有返回值ScheduledFuture。可以通过该对象获取线程信息。
     * @param runnable     任务
     * @param initialDelay 执行延迟
     * @param period       执行周期
     * @return 任务
     */
    public void executeAtFixedTask(Runnable runnable, long initialDelay, long period) {
        getScheduledThreadPoolExecutor().scheduleAtFixedRate(runnable, initialDelay, period, TimeUnit.MILLISECONDS);
    }

    /**
     * 与scheduleAtFixedRate方法类似，不同的是第三个参数表示前一次结束的时间和下一次任务启动的间隔时间。
     * @param runnable     任务
     * @param initialDelay 执行延迟
     * @param period       前一次结束的时间和下一次任务启动的间隔时间
     * @return
     */
    public void executeWithFixedTask(Runnable runnable, long initialDelay, long period) {
        getScheduledThreadPoolExecutor().scheduleWithFixedDelay(runnable, initialDelay, period, TimeUnit.MILLISECONDS);
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






