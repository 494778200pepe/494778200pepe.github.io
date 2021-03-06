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
public class User implements Parcelable{
    public int userId;
    public String userName;
    public String password;
    public Book book;

    public User(int userId,String userName,String password,Book book){
        this.userId=userId;
        this.userName=userName;
        this.password=password;
        this.book=book;
    }

    public int describeContents(){
        //几乎所有情况都返回0，仅在当前对象中存在文件描述符时返回1
        return 0;
    }
    
    // 将对象中的属性保存至目标对象out中  
    public void writeToParcel(Parcel out,int flags){
        out.writeInt(userId);
        out.writeString(userName);
        out.writeString(password);
        out.writeParcelable(book,0);
    }

    //用来创建自定义的Parcelable的对象
    public static final Parcelable.Creator<User> CREATOR=new Parcelable.Creator<User>(){
    
        //重写createFromParcel方法，创建并返回一个获得了数据的MyParcelable对象  
        public User createFromParcel(Parcel in){
            return new User(in);
        }

        public User[] newArray(int size){
            return new User[size];
        }
    }

    // 读数据进行恢复，带参构造器方法私用化，本构造器仅供类的方法createFromParcel调用  
    private User(Parcel in){
        userId=in.readInt();
        userName=in.readString();
        password=in.readString();       
        book=in.readParcelable(Thread.currentThread().getContextClassLoader());
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

### **Parcelable**

> Parcelable接口是Android SDK提供的一种专门用于Android应用中对象的序列化和反序列化的方式，相比于Seriablizable具有更好的性能。实现Parcelable接口的对象就可以实现序列化并可以通过Intent和Binder传递。

### **Parcelable相关方法说明**

|方法--------------------------------|功能----------------------------------------|标记位|
|:-----------------------------------|:-------------------------------------------|:---- |
|createFromParcel(Parcel in)	     |从序列化后的对象中创建原始对象		      ||
|newArray(int size)	                 |创建指定长度的原始对象数组                  ||
|User(Parcel in)	                 |从序列化后的对象中创建原始对象	          ||
|writeToParcel(Parcel out,int flags) |将当前对象写入序列化结构中	                |PARCALABLE_WRITE_RETURN_VALUE|
|describeContents	                 |返回当前对象的内容描述，几乎所有情况都返回0，仅在当前对象中存在文件描述符时返回1|CONTENTS_FILE_DESCRIPTOR|

### **Parcelable和Serializable的区别**

|区别------------|Serializable--------------------------------|Parcelable|
|:---------------|:-------------------------------------------|:---- |
|所属API	     |JAVA API	                                  |Android SDK API|
|原理	         |序列化和反序列化过程需要大量的I/O操作       |序列化和反序列化过程不需要大量的I/O操作|
|开销	         |开销大	                                  |开销小|
|效率	         |低	                                      |很高|
|使用场景	     |序列化到本地或者通过网络传输	              |内存序列化|

### **效率和选择**

 - Parcelable的性能比Serializable好，在内存开销方面较小，所以在内存间数据传输时推荐使用Parcelable，如activity间传输数据。
 - 而Serializable可将数据持久化方便保存，所以在需要保存或网络传输数据时选择Serializable 
 - 因为android不同版本Parcelable可能不同，所以不推荐使用Parcelable进行数据持久化


参考：

[Android中两种序列化方式的比较Serializable和Parcelable - CSDN博客](https://blog.csdn.net/wangchunlei123/article/details/51345130)

[Android Serializable与Parcelable原理与区别 - CSDN博客](https://blog.csdn.net/androiddevelop/article/details/22108843?utm_source=tuicool&utm_medium=referral)