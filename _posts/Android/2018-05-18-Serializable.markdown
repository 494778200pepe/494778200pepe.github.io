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

* 如果在序列化写 时的版本号和序列化读 时的版本号，不一致，将会有异常：`java.io.InvalidClassException`。
* `serialVersionUID`用于在反序列化时，做校验。比如网络传输时，serialVersionUID不一致，反序列化就不能正常进行。
* 如果在`class`中不声明`serialVersionUID`，那么在序列化写 的时候，虚拟机会自动为它计算出一个`serialVersionUID`，计算的方法是依据`class`的信息。
* 如果两个虚拟机是不同类型的虚拟机，那么计算方法可能就不一样了，于是即使相同的`class`，`serialVersionUID`也可能会不同。
* 所以最好自己指定 `serialVersionUID`。

[关于Serializable的serialVersionUID - CSDN博客](https://blog.csdn.net/smcwwh/article/details/8787561)

### **class发生变化**

 - 1）、如果虚拟机A中的AClass有一个属性，而虚拟机B中的AClass，没有这个属性，那么这个属性将被忽略，而不会有异常。

 - 2）、如果虚拟机A中的AClass没有的属性，而在虚拟机B中多出来的属性，那么这个属性将被赋予一个缺省值，而不会有异常。

 - 3）、如果虚拟机A中的AClass有一个属性，在虚拟机B中的AClass也有这个属性，但这个属性的类型变了，比如说int变成了long，抑或其他的变化，将会有异常：`java.io.InvalidClassException:incompatible types for field …`

经过序列化而产生的异常都是 `java.io.InvalidClassException`，不会产生`java.lang.ClassCastException`，两者还是有比较大的区别的，从名字上就可以看得出来。

### **静态变量序列化**

* 串行化只能保存对象的非静态成员交量，不能保存任何的成员方法和静态的成员变量.
* 而且串行化保存的只是变量的值，对于变量的任何修饰符都不能保存。

### **transient关键字**

> transient关键字的作用是：阻止实例中那些用此关键字声明的变量持久化；当对象被反序列化时（从源文件读取字节序列进行重构），这样的实例变量值不会被持久化和恢复。
> 当某些变量不想被序列化，同是又不适合使用static关键字声明，那么此时就需要用transient关键字来声明该变量。

### **序列化中的继承问题**

* 当一个父类实现序列化，子类自动实现序列化，不需要显式实现Serializable接口。
* 一个子类实现了 Serializable 接口，它的父类都没有实现 Serializable 接口，要想将父类对象也序列化，就需要让父类也实现Serializable 接口。

第二种情况中：如果父类不实现 Serializable接口的话，就需要有默认的无参的构造函数。这是因为一个 Java 对象的构造必须先有父对象，才有子对象，反序列化也不例外。在反序列化时，为了构造父对象，只能调用父类的无参构造函数作为默认的父对象。因此当我们取父对象的变量值时，它的值是调用父类无参构造函数后的值。在这种情况下，在序列化时根据需要在父类无参构造函数中对变量进行初始化，否则的话，父类变量值都是默认声明的值，如 int 型的默认是 0，string 型的默认是 null。

参考：

[Serializable - CSDN博客](https://blog.csdn.net/u011568312/article/details/57611440)

[Java Serializable系列化与反系列化 - CSDN博客](https://blog.csdn.net/smcwwh/article/details/7032764)

[实现Serializable的单例模式 | 学步园](http://www.xuebuyuan.com/1984718.html)
