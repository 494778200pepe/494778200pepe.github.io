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
```
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
参考：

[Android中两种序列化方式的比较Serializable和Parcelable - CSDN博客](https://blog.csdn.net/wangchunlei123/article/details/51345130)