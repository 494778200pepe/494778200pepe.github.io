---
layout: post
title:  "Serializable"
date:   2018-05-18 08:47:00 +0800
categories: Android
tags: Android
author: pepe
description: 『 Serializable 』
---

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
### **序列化**

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

### **用途**

* 想把的内存中的对象状态保存到一个文件中或者数据库中时候
* 想把对象通过网络进行传播的时候

### **serialVersionUID**

* 如果在序列化写 时的版本号和序列化读 时的版本号，不一致，将会有异常：java.io.InvalidClassException。
* serialVersionUID用于在反序列化时，做校验。比如网络传输时，serialVersionUID不一致，反序列化就不能正常进行。
* 如果在class中不声明serialVersionUID，那么在序列化写 的时候，虚拟机会自动为它计算出一个serialVersionUID，计算的方法是依据class的信息。
* 如果两个虚拟机是不同类型的虚拟机，那么计算方法可能就不一样了，于是即使相同的class，serialVersionUID也可能会不同。
* 所以最好自己指定 serialVersionUID。

[关于Serializable的serialVersionUID - CSDN博客](https://blog.csdn.net/smcwwh/article/details/8787561)

### **class发生变化**

 - 1）、如果虚拟机A中的AClass有一个属性，而虚拟机B中的AClass，没有这个属性，那么这个属性将被忽略，而不会有异常。

 - 2）、如果虚拟机A中的AClass没有的属性，而在虚拟机B中多出来的属性，那么这个属性将被赋予一个缺省值，而不会有异常。

 - 3）、如果虚拟机A中的AClass有一个属性，在虚拟机B中的AClass也有这个属性，但这个属性的类型变了，比如说int变成了long，抑或其他的变化，将会有异常：java.io.InvalidClassException:incompatible types for field …

经过序列化而产生的异常都是 `java.io.InvalidClassException`，不会产生`java.lang.ClassCastException`，两者还是有比较大的区别的，从名字上就可以看得出来。


参考：

[Android中两种序列化方式的比较Serializable和Parcelable - CSDN博客](https://blog.csdn.net/wangchunlei123/article/details/51345130)