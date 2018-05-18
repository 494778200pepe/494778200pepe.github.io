---
layout: post
title:  "Serializable和Parcelable"
date:   2018-05-18 08:47:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 Serializable和Parcelable 』
---

> Serializable和Parcelable接口可以完成对象的序列化过程，当我们需要通过Intent和Binder传输数据时就需要使用者两种序列化方式。

### **Serializable的实现**
对于Serializable，类只需要实现Serializable接口，并提供一个序列化版本id(serialVersionUID)即可。
```
public class MySerializable implements Serializable{
    private static final long serialVersionUID = 1L;
    private int mData;
    private String mStr;

    public int getmData() {
        return mData;
    }

    public void setmData(int mData) {
        this.mData = mData;
    }

    public String getmStr() {
        return mStr;
    }

    public void setmStr(String mStr) {
        this.mStr = mStr;
    }
}

#### 说明

> 序列化：对象的寿命通常随着生成该对象的程序的终止而终止，有时候需要把在内存中的各种对象的状态（也就是实例变量，不是方法）保存下来，并且可以在需要时再将对象恢复。虽然你可以用你自己的各种各样的方法来保存对象的状态，但是Java给你提供一种应该比你自己的好的保存对象状态的机制，那就是序列化。

总结：Java 序列化技术可以使你将一个对象的状态写入一个Byte 流里（系列化），并且可以从其它地方把该Byte 流里的数据读出来（反序列化）。
```
//序列化到本地
User user=new User(0,"wcl_android@163.com","123456");
ObjectOutputStream out=new ObjectOutputStream(new FileOutputStream("user.obj"));
out.writeObject(user);
out.close;

//反序列化
ObjectInputStream in=new ObjectInputStream(new FileInputStream("user.obj"));
User user=(User)in.readObject();
in.close();
```



参考：

[Android中两种序列化方式的比较Serializable和Parcelable - CSDN博客](https://blog.csdn.net/wangchunlei123/article/details/51345130)