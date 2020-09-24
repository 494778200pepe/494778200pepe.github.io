---
layout: post
title:  "Kotlin -  与 Java 代码互调"
date:   2020-09-10 10:22:00 +0800
categories: Kotlin
tags: Kotlin
author: pepe
description: 『  与 Java 代码互调 』
---

### 语法变化

kotlin的函数可以直接写在文件里面，而不用写在类里面
在 jvm 编译之后，都会生成  public static 的方法或者变量

```
// Utils.kt
fun echo(name:String){
	println("$name")
} 

// Main.java
public static void main(String[] args){
	// 可以通过 @file:JvmName("UtilsCode")  修改最终生成的 class 文件名
	// 这样使用的时候，就是 UtilsCode.echo("hello");
	// 参考：Kotlin 自定义 kt 文件类名 - 简书
			https://www.jianshu.com/p/b64fe377e08a
	UtilsKt.echo("hello");
}
```

#### 创建匿名内部类

```
object Test{
	fun sayMessage(msg:String){
		println(msg)
	}
}
// 会被编译成一个类，里面有一个 INSTANCE 的单例对象

kotlin code Test.sayMessage("hello")

java code  	Test.INSTANCE.sayMessage("hello");
```
 
#### 传 class 参数

```
java code 	TestMain.class
kotlin code	TestMain::class.java
// kotlin 和 java 的  class 格式是不一致的。


fun textClass(clazz:Class<JavaMain>){
	println(clazz.simpleName)
}

fun textClass(clazz:KClass<KotlinMain>){
	println(clazz.simpleName)
}
```


### kotlin关键字处理

在 kotlin 里面 in 是关键字
```
// JavaMain.java
public class JavaMain{
	public static String in = "in";
}


// Test.kt
fun main(args:Array<String>){
	// 添加两个反引号，解决关键字冲突
	println(JavaMain.`in`)
}


```

### 基本数据类型的处理









