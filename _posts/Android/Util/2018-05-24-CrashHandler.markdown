---
layout: post
title:  "CrashHandler"
date:   2018-05-24 15:47:00 +0800
categories: Android
tags: Util
author: pepe
description: 『 异常拦截器 』
---

```
import android.content.Context;
import android.os.Process;
import android.util.Log;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.lang.Thread.UncaughtExceptionHandler;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.List;

/**
 * Crash
 * 异常拦截器
 * Created by wang on 2017/4/13.
 */
public class CrashHandler implements UncaughtExceptionHandler {

    public static final String TAG = "HqCameraSystem";
    private static CrashHandler instance;
    private Context context;
    private String errorLogDir = null;
    private SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd_HH");
    private SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    private final long expirationDate = 7;//设置过期时间(单位：天)

    private CrashHandler() {
    }

    public synchronized static CrashHandler getInstance() {
        if (instance == null) {
            instance = new CrashHandler();
        }
        return instance;
    }

    public void init(Context context) {
        this.context = context;
        this.errorLogDir = "mnt/sdcard";
        Thread.setDefaultUncaughtExceptionHandler(this);
        //删除过期文件
        new Thread(new Runnable() {
            @Override
            public void run() {
                deleteExpirationFile();
            }
        }).start();
    }

    @Override
    public void uncaughtException(Thread thread, Throwable ex) {
        if (ex != null) {
            disposeThrowable(ex);
            ex.printStackTrace();
        }
    }

    private void disposeThrowable(Throwable ex) {
//        StringBuffer sb = new StringBuffer();
        StringBuilder sb = new StringBuilder();
        sb.append(sdf2.format(System.currentTimeMillis()) + "\n");
        sb.append(ex.toString() + "\n");
        StackTraceElement[] steArray = ex.getStackTrace();
        for (StackTraceElement ste : steArray) {
            sb.append("System.err at " + ste.getClassName() + "." + ste.getMethodName() + "(" + ste.getFileName() + ":" + ste.getLineNumber() + ")\n");
        }
        if (errorLogDir != null) {
            FileOutputStream fos = null;
            try {
                File file = new File(errorLogDir, "error_" + sdf.format(System.currentTimeMillis()) + "_camera.txt");
                if (!file.exists()) {
                    file.getParentFile().mkdirs();
                    file.createNewFile();
                }
                fos = new FileOutputStream(file);
                fos.write(sb.toString().getBytes("utf-8"));
                fos.flush();

            } catch (FileNotFoundException e) {
                Log.d(TAG, "error ： " + e);
            } catch (IOException e) {
                Log.d(TAG, "error ： " + e);
            } finally {
                try {
                    if (fos != null) {
                        fos.close();
                    }
                } catch (IOException e) {
                    Log.d(TAG, "error ： " + e);
                }
            }
        }
        Process.killProcess(Process.myPid());
//        System.exit(10);
    }

    private void deleteExpirationFile() {
        if (errorLogDir == null) {
            return;
        }
        long expirationTime = expirationDate * 24 * 3600 * 1000;
        long nowTime = System.currentTimeMillis();
        List<File> deleteList = new ArrayList<File>();
        File dir = new File(errorLogDir);
        if (dir.isDirectory()) {
            File[] files = dir.listFiles();
            for (File f : files) {
                if (f.exists()) {
                    long time = f.lastModified();
                    if (nowTime - time >= expirationTime) {
                        deleteList.add(f);
                    }
                }
            }
            for (File f : deleteList) {
                f.delete();
            }
        }
    }
}
```

参考：

[Android 重写系统Crash处理类，保存Crash信息到SD卡 和 完美退出程序的方法 - CSDN博客](https://blog.csdn.net/xiaanming/article/details/9344703)



