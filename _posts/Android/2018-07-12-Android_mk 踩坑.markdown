---
layout: post
title:  "Android_mk 踩坑"
date:   2018-07-12 13:43:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 Android_mk 踩坑 』
---

### **如何添加在mk文件中配置多个 aidl**
```
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)
#声明多个 jar 包的位置
LOCAL_PREBUILT_STATIC_JAVA_LIBRARIES := hqlib:../../../libs/hqlib.jar \
										hqapi:../../../libs/hqapi.jar \
										rxjava:../../../libs/rxjava-1.3.7.jar

LOCAL_PREBUILT_STATIC_JAVA_LIBRARIES += rxandroid:../../../libs/rxandroid-1.2.1.aar \
                                        vitamio:../../libs/vitamio.aar
										
include $(BUILD_MULTI_PREBUILT)
include $(CLEAR_VARS)
LOCAL_PACKAGE_NAME := HQ_VideoPlayer

#指该模块在所有版本下都编译
LOCAL_MODULE_TAGS := optional

#混淆配置
#LOCAL_PROGUARD_ENABLED := full obfuscation
#LOCAL_PROGUARD_FLAG_FILES := ../../proguard-rules.pro

#设置不打odex包
LOCAL_DEX_PREOPT := false
DONT_DEXPREOPT_PREBUILTS := true  

#apk路径
LOCAL_MODULE_PATH := $(TARGET_OUT)/app/kl/TenPurple

LOCAL_CERTIFICATE := platform

LOCAL_MULTILIB := 32

LOCAL_RESOURCE_DIR += $(LOCAL_PATH)/res

src_dirs := java/
LOCAL_SRC_FILES := $(call all-java-files-under, $(src_dirs))

LOCAL_SRC_FILES +=  $(call all-Iaidl-files-under, aidl)
LOCAL_AIDL_INCLUDES := $(LOCAL_PATH)/aidl

#引用我们声明的多个 jar 包的变量
LOCAL_STATIC_JAVA_LIBRARIES +=  hqlib \
                                hqapi \
                                rxjava \
                                android-support-v4
                                
LOCAL_STATIC_JAVA_AAR_LIBRARIES += vitamio \
								   rxandroid

LOCAL_AAPT_FLAGS += --auto-add-overlay \
                    --extra-packages io.vov.vitamio \
					--extra-packages rx.android
version_code = 1
version_name := 1.0.0.1
LOCAL_AAPT_FLAGS += --version-code $(version_code)
LOCAL_AAPT_FLAGS += --version-name $(version_name)   

LOCAL_SDK_VERSION := 19  

include $(BUILD_PACKAGE)
```
[Android Aidl Compile Error: couldn't find import for class - Stack Overflow](https://stackoverflow.com/questions/27138924/android-aidl-compile-error-couldnt-find-import-for-class)



