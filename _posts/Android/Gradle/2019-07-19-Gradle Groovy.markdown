---
layout: post
title:  "Gradle Groovy"
date:   2019-07-19 15:45:00 +0800
categories: Android
tags: Gradle
author: pepe
description: 『 Groovy 』
---

### **Groovy安装**

##### (1)下载地址

[The Apache Groovy programming language - Download](http://www.groovy-lang.org/download.html)

##### (2)设置环境变量
```
GROOVY_HOME=E:\developer\groovy-2.4.8
PATH=$PATH;%GROOVY_HOME%\bin
```
#### (3)在cmd命令中运行
```
> groovy -v
Groovy Version: 2.4.8 JVM: 1.8.0_25 Vendor: Oracle Corporation OS: Windows 7
```

### **基础语法**

#### **定义变量**

```
// Groovy使用def来声明变量，声明变量可以不加类型，运行的时候可以自动推断出变量类型，并且声明语句可以不加分号结尾
def  i1 = 10
def  str1 = "hello groovy"
def  d1 = 1.25
```

#### **定义函数**

```
// Groovy中函数返回值可以是无类型的
def showName1(){
    println  "wang lei 1" //最后一行执行结果默认作为函数返回值，可以不加return
}
showName1()

// 如果我们指定了函数的返回值类型，那么可以不加def关键字
String showName2(){
    "println wang lei 2" //最后一行执行结果默认作为函数返回值
}
println showName2()
```

#### **字符串**

Groovy 中字符串分为三种，我们一个个来看

```
// 单引号字符串，单引号字符串不会对$进行转义，原样输出
def i2 = 10
def str2 = 'i am $i2 yuan'//单引号不会转义
println str2 //输出  i am $i2 yuan

// 双引号字符串，双引号字符串会对$号进行转义
def i3 = 10
def str3 = "i am $i3 yuan"//双引号会转义
println str3 //输出  i am 10 yuan

// 三引号字符串，三引号字符串支持换行，原样输出
// 三个引号原样输出
def str4 = '''
    public static void main(){
        println "miss you"
    }
'''
println str4
```


#### **数据类型**
```
// 数据类型，int，double等Java中的基本数据类型，在Groovy中对应的是它们的包装数据类型。比如int对应为Integer。
def x1 = 23
println x1.getClass().getCanonicalName()//java.lang.Integer

def f1 = true
println f1.getClass().getCanonicalName()//java.lang.Boolean
```

#### **List**

```
println '==> List'
def myList = [5,'werw',true]//看到了吧，可以放任意数据，就是那么任性
myList.add(34)
//myList.add(12,55)//这样会报角标越界异常
myList[6] = true
println myList.size()
//遍历
myList.each{
    println "item: $it"
}
myList.eachWithIndex {
    it, i5 -> // `it` is the current element,`i5` is the index
    println "$i5: $it"
}
println myList.getClass().getCanonicalName()
```

#### **map**

```
println '==> map'
//println 'Map类'
def map = ['key1':true,name:"str1",3:"str3",4:"str4"]
println map.name // 相当于 map.get('name')
println map.get(4)
map.age = 14 //加入 age:14 数据项
println map.age //输出14
println map.getClass().getCanonicalName()
//遍历
map.each{
    key,value -> 
       println key +"--"+ value
    }

map.each{
    it -> 
       println "$it.key ::: $it.value"
    }
// `entry` is a map entry, `i6` the index in the map
map.eachWithIndex { entry, i6 ->
    println "$i6 - Name: $entry.key Age: $entry.value"
}

// Key, value and i7 as the index in the map
map.eachWithIndex { key, value, i7 ->
    println "$i7 - Name: $key Age: $value"
}

println map.containsKey('key1')  //判断map中是否包含给定的key
println map.containsValue(1)  //判断map中是否包含给定的value
//返回所有
println map.findAll{
    entry ->
         entry.value instanceof String//value是String类型的
}
//返回第一个符合要求的
println map.find{
    entry ->
        entry.key instanceof Integer && entry.key > 1
} //返回符合条件一个entry

//清空
map.clear()

println map.size()
```

#### **Range**

```
println '==> Range'
def Range1 = 1..10//包括10
println Range1.contains(10)
println Range1.from +"__"+Range1.to
def Range2 = 1..<10//不包括10
println Range2.contains(10)
println Range2.from +"__"+Range2.to

//遍历
for (j in 1..10) {
    println "for ${j}"
}

(1..10).each { k ->
    println "each ${k}"
}
def years = 23
def interestRate
switch (years) {
    case 1..10: interestRate = 0.076; break;
    case 11..25: interestRate = 0.052; break;
    default: interestRate = 0.037;
}
println interestRate//0.052
```

#### **闭包**

> 闭包 Closure 一开始接触的时候真是有点蒙圈的感觉，这是什么玩意，Kotlin 中也有这玩意，接触多了也就慢慢习惯了，其实就是一段可执行的代码，我们可以像定义变量一样定义可执行的代码。

##### 定义闭包

闭包一般定义如下：
```
def xxx = {         // xxx 闭包名
    paramters ->    // paramters 参数名
        code
}
```
比如，我们定义一下闭包：
```
def mClosure = {
    p1,p2 ->
         println p1 +"..."+p2
}
```
p1,p2也可以指定数据类型，如下：
```
// 闭包定义
// ->前是参数定义， 后面是代码
def mClosure = {
    String p1, int p2 ->
         println p1 +"..."+p2
}
```
##### 调用闭包

调用上述定义的闭包：
```
// 调用 均可以
mClosure("qwewe",100)
mClosure.call("qwewe",100)
```
看到这里有C经验的是不是瞬间想到函数指针了，连调用都非常像。
定义闭包我们也可以不指定参数，如不指定则默认包含一个名为 `it` 的参数：
```
def hello = { "Hello, $it!" }
println hello("groovy")//Hello, groovy!
```
##### 闭包作为方法参数

注意一点，在 `Groovy` 中，当函数的最后一个参数是闭包或者只有一个闭包参数，可以省略调用时的圆括号。比如：
```
def fun(Closure closure){
    println "fun1"
    closure()
}
//调用方法
fun({
    println "closure"
})
//可以不写()直接调用
fun{
    println "closure"
}
```
或者如下：
```
def fun(int a , Closure closure){
    println "fun1 $a"
    closure()
}
//调用方法
fun(1, {
    println "closure"
})
//可以不写()直接调用，这就有点奇葩了
fun 1, {
    println "closure"
}
```
关于闭包，刚接触肯定不习惯，自己一定要花时间慢慢体会，闭包在Gradle中大量使用，后面讲Gradle的时候会大量接触闭包的概念。



#### **读文件**

读文件有三种操作：

* 一次读取一
* 读取全部返回字节数据
* 数据流的形式读取

```
// 一次读取一行：

def src = new File("C:\\Users\\wanglei55\\Desktop\\Groovy\\in.txt")
// 每次读取一行
src.eachLine{
    //it就是每一行数据
    it ->
        println it
}
// 读取全部返回字节数据：

// 一次读取全部：字节数组
def bytes = src.getBytes()
def str = new String(bytes)
println "str::"+str
// 数据流的形式读取：

// 返回流，不用主动关闭
src.withInputStream{
    inStream ->
        //操作。。。。
}
```

#### **写文件**

```
//写数据
def des = new File("C:\\Users\\wanglei55\\Desktop\\Groovy\\out.txt")
des.withOutputStream{ 
    os->
        os.leftShift(bytes)//左移在这里可以起到写入数据的作用
}

// 我们也可以逐行写入数据：

des.withWriter('utf-8') { writer ->
    writer.writeLine 'i'
    writer.writeLine 'am'
    writer.writeLine 'wanglei'
}

// 我们也可以这样写入数据：
des << '''i love groovy'''
```

#### **XML解析**

在我电脑有xml文档，内容如下：
```
<response version-api="2.0">
    <value>
        <persons>
            <person id = "1">
                <name>zhangsan</name>
                <age>12</age>
            </person>
            <person id = "2">
                <name>lisi</name>
                <age>23</age>
            </person>
        </persons>
    </value>
</response>
```

Groovy 解析 xml 文档同样非常简单，我们可以像操作对象一样操作 xml：
```
// xml解析
def xmlFile = new File("C:\\Users\\wanglei55\\Desktop\\Groovy\\test.xml")
def parse = new XmlSlurper()
def result =parse.parse(xmlFile)

def p0 = result.value.persons.person[0]
println p0['@id']
println p0.name
println p0.age
```
很简单，没什么要特别说明的，这里稍微记一下，后面写插件的时候会解析安卓的AndroidManifest.xml文件

#### **Json**
直接看代码吧，很简单：
```
// Person类
class Person{
    def first
    def last
}

def p = new Person(first: 'wanglei',last: '456')
// 转换对象为json数据
def jsonOutput = new JsonOutput()
def json = jsonOutput.toJson(p)
println(json)
// 格式化输出
println(jsonOutput.prettyPrint(json))

// 解析json
def slurper = new JsonSlurper()
Person person = slurper.parseText(json)
println person.first
println person.last
```
打印输出如下：
```
{"first":"wanglei","last":"456"}
{
    "first": "wanglei",
    "last": "456"
}
wanglei
456
```







参考：

[Gradle入门到实战(一) — 全面了解Gradle](https://mp.weixin.qq.com/s?__biz=Mzg2NzAwMjY4MQ==&mid=2247483789&idx=1&sn=4b3bb2ab721c8ed7e05f1e8b2e0fbf70&chksm=ce4371dbf934f8cd7c484e8c5356d299bbd5d7790ee11bb0da9725068fa8e4b895f87379949f&token=655420148&lang=zh_CN#rd)

[The Apache Groovy programming language - The Groovy Development Kit](http://www.groovy-lang.org/groovy-dev-kit.html)

[The Apache Groovy programming language - Groovy Development Kit](http://www.groovy-lang.org/api.html)

[Gradle从入门到实战 - Groovy基础_ADC-Android API Android SDK Android Studio](https://www.android-doc.com/androidstudio/2017/0725/933.html)

[Groovy 使用完全解析 - CSDN博客](https://blog.csdn.net/zhaoyanjun6/article/details/70313790/#groovy-￥ﾮﾹ￥ﾙﾨ)
