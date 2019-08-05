---
layout: post
title:  "Gradle 自定义 task"
date:   2019-08-05 15:37:00 +0800
categories: Android
tags: Gradle
author: pepe
description: 『 自定义 task 』
---

task，如其名：任务，gradle就是由一个一个任务来完成的。他其实也是一个类，有自己的属性，也可以”继承”，甚至他还有自己的生命周期。

### **创建 task**

方式一：
```
class MyTask extends DefaultTask {

    String message = 'This is MyTask'

    // @TaskAction 表示该 Task 要执行的动作,即在调用该 Task 时，hello() 方法将被执行
    @TaskAction
    def hello() {
        println "Hello world. $message"
    }

    @Override
    String getGroup() {
        return "myGroup"
    }
}

// myTask1 使用了默认的 message 值
task myTask1(type: MyTask, group: 'mygroup')

// 重新设置了 message 的值
task myTask2(type: MyTask) {
    message = "I am an android developer"
}
// 任务(hello)开始前执行
myTask1.doFirst {
    println "Hello world. doFirst"
}
// 任务(hello)结束后执行
myTask1.doLast {
    println "Hello world. doLast"
}
// 配置阶段执行
myTask1.configure{
    println "Hello world. configure"
}
```

方式二：
```
task aTask(group: 'myGroup') {

    // 配置阶段执行
    println ">>>>>>>>>>>>>>>>config..."
    
    //任务开始执行初期执行
    doFirst {
        println ">>>>>>>>>>>>>>>>aTask doFirst..."
    }

    setGroup("mygroup")

    //任务开始执行末期执行
    doLast {
        println ">>>>>>>>>>>>>>>>aTask doLast..."
    }
}
```

### **参数设置**

定义Task的时候是可以指定很多参数的，如下所示：

<table width="800" border="2" cellspacing="0" cellpadding="2">
<tbody>
    <tr>
        <td>参数</td>
        <td>含义</td>
        <td>默认值</td>
    </tr>
    <tr>
        <td><font color="Hotpink">name</font></td>
        <td>task的名字</td>
        <td>不能为空，必须指定</td>
    </tr>
   <tr>
        <td><font color="Hotpink">type</font></td>
        <td>task的“父类”</td>
        <td>DefaultTask</td>
    </tr>
    <tr>
        <td><font color="Hotpink">overwrite</font></td>
        <td>是否替换已经存在的task</td>
        <td>false</td>
    </tr>
    <tr>
        <td><font color="Hotpink">dependsOn</font></td>
        <td>task依赖的task的集合</td>
        <td>[]</td>
    </tr>
    <tr>
        <td><font color="Hotpink">group</font></td>
        <td>task属于哪个组</td>
        <td>null</td>
    </tr>
    <tr>
        <td><font color="Hotpink">description</font></td>
        <td>task的描述</td>
        <td>null</td>
    </tr>
</tbody>
</table>

### **API**

[Task (Gradle API 5.5.1)](https://docs.gradle.org/current/javadoc/org/gradle/api/Task.html)

[TaskContainer (Gradle API 5.5.1)](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/TaskContainer.html)

参考：

[全面理解Gradle - 定义Task - 任玉刚 - CSDN博客](https://blog.csdn.net/singwhatiwanna/article/details/78898113)

[从Android Plugin源码开始彻底理解gradle构建：Task（三） - verymrq的博客 - CSDN博客](https://blog.csdn.net/verymrq/article/details/80482895)

[Android Gradle 自定义Task 详解 - 赵彦军 - 博客园](https://www.cnblogs.com/zhaoyanjun/archive/2017/12/05/7988965.html)

[Gradle技术之四 - Gradle的Task详解 - 简书](https://www.jianshu.com/p/9d727baed0f1)










 