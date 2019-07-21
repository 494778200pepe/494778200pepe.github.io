---
layout: post
title:  "Proguard混淆 GSON的序列化与反序列化"
date:   2019-07-21 11:36:00 +0800
categories: Android
tags: 混淆
author: pepe
description: 『 GSON的序列化与反序列化 』
---

GSON是一个很好的工具,使用它我们可以轻松的实现序列化和反序列化.但是当它一旦遇到混淆,就需要我们注意了.

一个简单的类Item,用来处理序列化和反序列化
```
public class Item {
    public String name;
    public int id;
}
```
序列化的代码
```
Item toSerializeItem = new Item();
toSerializeItem.id = 2;
toSerializeItem.name = "Apple";
String serializedText = gson.toJson(toSerializeItem);
Log.i(LOGTAG, "testGson serializedText=" + serializedText);
```
开启混淆之后的日志输出结果
```
I/MainActivity: testGson serializedText={"a":"Apple","b":2}
属性名已经改变了,变成了没有意思的名称,对我们后续的某些处理是很麻烦的.
```
反序列化的代码
```
Gson gson = new Gson();
Item item = gson.fromJson("{\"id\":1, \"name\":\"Orange\"}", Item.class);
Log.i(LOGTAG, "testGson item.id=" + item.id + ";item.name=" + item.name);
```
对应的日志结果是
```
I/MainActivity: testGson item.id=0;item.name=null
```
可见,混淆之后,反序列化的属性值设置都失败了.

### **为什么呢?**

* 因为反序列化创建对象本质还是利用反射,会根据 json 字符串的 key 作为属性名称,value 则对应属性值.

### **如何解决**

* 将序列化和反序列化的类排除混淆
* 使用@SerializedName注解字段

@SerializedName(parameter)通过注解属性实现了

* 序列化的结果中,指定该属性key为parameter的值.
* 反序列化生成的对象中,用来匹配key与parameter并赋予属性值.

一个简单的用法为
```
public class Item {
    @SerializedName("name")
    public String name;
    @SerializedName("id")
    public int id;
```

参考：

[关于Android代码混淆的详细讲解 - 码农教程](http://www.manongjc.com/article/1598.html)

