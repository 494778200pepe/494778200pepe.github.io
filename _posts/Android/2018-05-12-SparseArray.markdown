---
layout: post
title:  "SparseArray"
date:   2018-05-12 17:28:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 SparseArray 』
---
### **构造**
从构造方法我们可以看出，它和一般的List一样，可以预先设置容器大小，默认的大小是10：
```
public SparseArray() {  
    this(10);  
}  
  
  
public SparseArray(int initialCapacity) {  
    ......  
}  
```

### **增**
它有两个方法可以添加键值对：
```
public void put(int key, E value)  
public void append(int key, E value)   
```
在存储数据的时候，是采用了二分法方式，以下是它采用二分法的源码：
```
private static int binarySearch(int[] a, int start, int len, int key) {  
    int high = start + len;  
    int low = start - 1;  
  
  
    while (high - low > 1) {  
        int guess = (high + low) / 2;  
  
  
        if (a[guess] < key) {  
            low = guess;  
            continue;  
        }  
        high = guess;  
    }  
  
  
    if (high == start + len)  
        return start + len ^ 0xFFFFFFFF;  
    if (a[high] == key) {  
        return high;  
    }  
    return high ^ 0xFFFFFFFF;  
} 
```
所以，它存储的数值都是按键值从小到大的顺序排列好的。

### **查**
它有两个方法可以取值：
```
public E get(int key)  
public E get(int key, E valueIfKeyNotFound) 
``` 
最后一个从传参的变量名就能看出，传入的是找不到的时候返回的值

#### 查看第几个位置的键：
```
public int keyAt(int index)  
```
#### 查看第几个位置的值：
```
public E valueAt(int index)  
```
#### 查看键所在位置
由于采用`二分法查`找键的位置，所以没有的话返回小于0的数值，而不是返回-1，这点要注意，返回的负数其实是表示它在哪个位置就找不到了，如果你存了5个，查找的键大于5个值的话，返回就是-6：
```
public int indexOfKey(int key)  
```
#### 查看值所在位置，没有的话返回-1：
```
public int indexOfValue(E value) 
```

### **删**
它有四个方法：
```
public void delete(int key)  
public void remove(int key)  
```
但其实，`delete`和`remove`的效果是一样的，`remove`方法中调用了`delete`方法，`remove`源码：
```

public void remove(int key) {  
        delete(key);  
}  

public void removeAt(int index)  
public void clear()  
```
最后一个就是清除全部 

### **改**
```
public void setValueAt(int index, E value)  
public void put(int key, E value)  
```

`put`方法还可以修改键值对，注意：如果键不存在，就会变为添加新键值对

### **其他**

`SparseArray`实现了`Cloneable`接口，还可以调用`clone`方法。

### SparseArrayCompat

`SparseArrayCompat`其实是一个map容器,它使用了一套算法优化了hashMap;

* 优点：可以节省至少50%的缓存.
* 缺点：有局限性只针对下面类型：`key: Integer; value: object`

参考：

[Android编程之SparseArray详解 - CSDN博客](https://blog.csdn.net/pi9nc/article/details/11352491)

[Android内存优化（使用SparseArray和ArrayMap代替HashMap） - CSDN博客](https://blog.csdn.net/u010687392/article/details/47809295)

[Android中的HashMap,ArrayMap和SparseArray - 简书](https://www.jianshu.com/p/aff3b8990ab3)

[SparseArray - Android SDK | Android Developers](http://www.android-doc.com/reference/android/util/SparseArray.html)

[GC: SparseArray - android.util.SparseArray (.java) - GrepCode Class Source](http://www.grepcode.com/file/repository.grepcode.com/java/ext/com.google.android/android/5.1.1_r1/android/util/SparseArray.java?av=f)