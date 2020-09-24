---
layout: post
title:  "Kotlin -  函数与 Lambda 闭包"
date:   2020-09-10 13:42:00 +0800
categories: Kotlin
tags: Kotlin
author: pepe
description: 『  函数与 Lambda 闭包 』
---


### 函数的特性语法

```
fun echo(name: String){
	println("$name")
}

fun echo(name: String = "wang pei"){
	println("$name")
}

fun echo(name:  String) = println("$name")

```

### 嵌套函数

```
// 用途：在某些条件下触发递归的函数，不希望被外部函数访问到的函数
fun function(){
	val str = "hello world"
	
	fun say(coutn: Int = 10){
		// 可以访问外部函数的局部变量
		println(str)
		if(cout > 10){
			say(cout -1)
		}
	}
	say()
}
```


### 扩展函数

// 可以静态的给一个类，扩展成员方法和成员变量
// 主要用途：对第三方 sdk 或者类，新增一些你需要的方法

```
// fun 类型.方法名
// FileReadWrite.kt
fun File.readText(charset: Charset = Charsets.UTF-8):String = readBytes().toString(charset)
```

### Lmabda 闭包

```
	// Lambda 表达式
    var thread = Thread({ -> Unit})
    thread.start()
	// 如果 Lambda 没有参数，可以省略箭头符号 ->
	var thread1 = Thread({ Unit })
    thread1.start()
    // 如果 Lambda 是函数的最后一个参数，可以将大括号放在小括号的外面
    var thread2 = Thread(){ Unit }
    thread2.start()
    // 如果函数只有一个参数且这个参数是 Lambda ，则可以省略小括号
    var thread3 = Thread{ Unit }
    thread3.start()
```

闭包
```
// 声明闭包
val echo = {name:String ->
    println(name)
}


fun main(args: Array<String>) {
    // 闭包
    echo.invoke("wang pei")
    echo("wang pei")
}
```




