---
layout: post
title:  "ColorState"
date:   2018-11-12 22:45:00 +0800
categories: Android
tags: xml
author: pepe
description: 『 ColorState 』
---

### **selector**

```
// res/color/button_text.xml
// In Java: R.color.filename
// In XML: @[package:]color/filename

<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android" >
    <item
        android:color="hex_color"
        android:state_pressed=["true" | "false"]
        android:state_focused=["true" | "false"]
        android:state_selected=["true" | "false"]
        android:state_checkable=["true" | "false"]
        android:state_checked=["true" | "false"]
        android:state_enabled=["true" | "false"]
        android:state_window_focused=["true" | "false"] />
</selector>
```


