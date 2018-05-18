---
layout: post
title:  "Parcelable"
date:   2018-05-18 10:11:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 Parcelable 』
---

### **Parcelable的实现**

 -  步骤 1：自定义实体类，实现Parcelable接口，重写其两个方法。
 -  步骤 2：该实体类必须添加一个常量CREATOR（名字大小写都不能使其他的），该常量必须实现Parcelable的内部接口：Parcelable.Creator，并实现该接口中的两个方法。

```
public class MyParcelable implements Parcelable {
    private int mData;
    private String mStr;
    
    // 将对象中的属性保存至目标对象dest中  
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeInt(mData);
        dest.writeString(mStr);
    }
    
    //用来创建自定义的Parcelable的对象
    public static final Creator<MyParcelable> CREATOR = new Creator<MyParcelable>() {
        @Override
        public MyParcelable createFromParcel(Parcel in) {
            return new MyParcelable(in);
        }

        //重写createFromParcel方法，创建并返回一个获得了数据的MyParcelable对象  
        public MyParcelable[] newArray(int size) {
            return new MyParcelable[size];
        }
    };

    // 读数据进行恢复，带参构造器方法私用化，本构造器仅供类的方法createFromParcel调用  
    protected MyParcelable(Parcel in) {
        mData = in.readInt();
        mStr = in.readString();
    }

    @Override
    public int describeContents() {
        return 0;
    }
    
    public String getmStr() {
        return mStr;
    }

    public void setmStr(String mStr) {
        this.mStr = mStr;
    }

    public int getmData() {
        return mData;
    }

    public void setmData(int mData) {
        this.mData = mData;
    }
}
```
从上面我们可以看出Parcel的写入和读出顺序是一致的。如果元素是list读出时需要先new一个ArrayList传入，否则会报空指针异常。如下：
```
list = new ArrayList<String>();
in.readStringList(list);
```
 PS: 在自己使用时，read数据时误将前面int数据当作long读出，结果后面的顺序错乱，报如下异常，当类字段较多时务必保持写入和读取的类型及顺序一致。
```
11-21 20:14:10.317: E/AndroidRuntime(21114): Caused by: java.lang.RuntimeException: Parcel android.os.Parcel@4126ed60: Unmarshalling unknown type code 3014773 at offset 164
```

### **效率和选择**

 - Parcelable的性能比Serializable好，在内存开销方面较小，所以在内存间数据传输时推荐使用Parcelable，如activity间传输数据。
 - 而Serializable可将数据持久化方便保存，所以在需要保存或网络传输数据时选择Serializable 
 - 因为android不同版本Parcelable可能不同，所以不推荐使用Parcelable进行数据持久化


参考：

[Android中两种序列化方式的比较Serializable和Parcelable - CSDN博客](https://blog.csdn.net/wangchunlei123/article/details/51345130)