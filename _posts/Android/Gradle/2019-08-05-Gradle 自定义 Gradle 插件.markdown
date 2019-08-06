---
layout: post
title:  "Gradle 自定义 Gradle 插件"
date:   2019-08-05 16:35:00 +0800
categories: Android
tags: Gradle
author: pepe
description: 『 自定义 Gradle 插件 』
---

Gradle中插件可以分为两类：脚本插件和对象插件。

### **脚本插件**

> 脚本插件就是一个普通的 gradle 构建脚本，通过在一个 `config.gradle` 脚本中定义一系列的 `task`，另一个构建脚本 `app.gradle` 通过 `apply from:'config.gradle' `即可引用这个脚本插件。

首先在项目根目录下新建一个 config.gradle 文件，在该文件中定义所需的 task。
```
//config.gradle
project.task("showConfig") {
     doLast {
        println("$project.name:showConfig")
    }
}
```
然后在需要引用的 module 的构建脚本中引用 `config.gradle`,例如在 `app.gradle` 中,由于 `config.gradle` 建立在根目录下，与 app 这个模块平级，所以需要注意路径问题 `../config.gradle`。
```
//app.gradle
apply from: '../config.gradle'
```
就是这么简单，此时运行 gradle 构建即可执行 showConfig 这个 task，
```
C:\c_project\MyApplication\app>gradle showConfig
> Task :app:showConfig
app:showConfig
```

### **对象插件**

> 对象插件是指实现了 `org.gradle.api.Plugin` 接口的类。Plugin 接口需要实现 `void apply(T target)` 这个方法。该方法中的泛型指的是此 Plugin 可以应用到的对象，而我们通常是将其应用到 Project 对象上。

编写对象插件主要有三种方式：

* 1、直接在gradle脚本文件中

* 2、在buildSrc目录下

* 3、在独立的项目下

#### **在gradle脚本文件中** 

直接在gradle脚本中编写这个方式是最为简单的。打开app.gradle文件，在其中编写一个类实现Plugin接口。
```
//app.gradle
class CustomPluginInBuildGradle implements Plugin<Project> {
    @Override
    void apply(Project target) {
        target.task('showCustomPluginInBuildGradle',group: 'myGroup'){
            doLast {
                println("task in CustomPluginInBuildGradle")
                println("task's group is $group")
            }
        }
    }
}

// 然后通过插件类名引用它
apply plugin: CustomPluginInBuildGradle
```
执行插件中定义的task
```
C:\c_project\MyApplication\app>gradle showCustomPluginInBuildGradle
task in CustomPluginInBuildGradle
task's group is myGroup
```

#### **在buildSrc目录下** 

![gradle1]({{ site.baseurl }}/assets/images/android/Gradle/gradle插件1.png)

* 1、创建一个Module，选择Java Library项目，项目名称必须是 buildSrc，否则插件不被识别

* 2、构建目录 buildSrc/src/main/groovy 本路径android studio会自动识别为 groovy类。 

* 3、在 main 目录中再构建 resources/META-INF/gradle-plugins，这个目录是groovy项目的资源文件目录。 

* 4、buildSrc 中的 build.gradle的定义，引入groovy插件，并依赖 gradleApi()、localGroovy()。

```
//  buildSrc/build.gradle
apply plugin: 'groovy'
dependencies {
    compile gradleApi()
    compile localGroovy()
}
```

* 5、代码实现
    
    。新建 groovy 文件 `MyPlugin.groovy`,代码如下
    
```
package com.pepe.plugin

import org.gradle.api.Plugin
import org.gradle.api.Project

class MyPlugin implements Plugin<Project> {

    @Override
    void apply(Project project) {
        System.out.println("========================")
        System.out.println("这是个插件!")
        System.out.println("========================")

        //增加闭包名称，闭包为customPlugin，是 CustomPluginTestExtension类型，因此CustomPluginTestExtension类型中的JaveBean类型的属性可以任意设置
        project.extensions.add("personInfo", PersonInfo)  //personInfo用于build.gradle中添加配置块
        project.task("showPersonInfo", group: 'mygroup').doLast {
            if (project.personInfo == null) return
            println("姓名：" + project.personInfo.name)
            println("年龄：" + project.personInfo.age)
            println("地址：" + project.personInfo.address)
        }
        project.extensions.add("bookInfo", BookInfo)
        project.task("showBookInfo", dependsOn: "showPersonInfo", group: 'mygroup').doLast {
            //注意，showBookInfo依赖showPersonInfo，dependsOn:"showPersonInfo"
            def book = project.extensions.findByType(BookInfo)
            println("喜欢的书籍：" + book.name + ", " + book.id + ", " + book.price + '元' + '，' + book.address + "," + book.isbn);
        }

        project.task('showStandAlonePlugin', group: 'mygroup') {
            doLast {
                println('task in StandAlonePlugin')
            }
        }
    }
}
```
    
    。PersonInfo类
    
```
package  com.pepe.plugin

class PersonInfo {
    def name = "init";
    def age = "init";
    def address = "init";
}
```
    
    。BookInfo类
    
```
package  com.pepe.plugin

class BookInfo {
    def name = "《红楼们》";
    def isbn = "SW.SH.CN.I.20181227";
    def address = "北京市海淀区西北旺";
    def price = 25.9f
    def id = 'BS1001029'
}
```
    
    。配置 `myplugin.properties`
    
```
implementation-class=com.ncf.plg.CustomPluginTest
```

> 注意：注意这里引用的方式可以是通过类名引用，也可以通过给插件映射一个id，然后通过id引用。

通过类名引用插件的需要使用全限定名，也就是需要带上包名，或者可以先导入这个插件类，如下
```
apply plugin: com.pepe.plugin.MyPlugin
或者
import com.pepe.plugin.MyPlugin
apply plugin: MyPlugin
```
通过简单的id的方式，我们可以隐藏类名等细节，使的引用更加容易。映射方式很简单，在buildSrc目录下创建 `resources/META-INF/gradle-plugins/xxx.properties`,这里的 `xxx`也就是所映射的 id，这里我们假设取名 `myplugin`。具体结构可参考上文 buildSrc 目录结构。

`myplugin.properties` 文件中配置该 id 所对应的 plugin 实现类

```
implementation-class=com.pepe.plugin.pepe
```
此时就可以通过id来引用对于的插件了
```
//app.gradle
apply plugin: 'myplugin'
```

#### **在独立工程下**

![gradle2]({{ site.baseurl }}/assets/images/android/Gradle/gradle插件2.png)

在 buildSrc 下创建的 plugin 只能在该工程下的多个模块之间复用代码。如果想要在多个项目之间复用这个插件，我们就需要在一个单独的工程中编写插件，将编译后的 jar 包上传 maven 仓库。

* 1、1.在build.gradle中添加uploadArchives 块

```
apply plugin:'groovy'  //必须
apply plugin: 'maven'  //要想发布到Maven，此插件必须使用

group='com.pepe.plugin'
version='3.0.0'

dependencies {
    implementation gradleApi() //必须
    implementation localGroovy() //必须
    implementation fileTree(dir: 'libs', include: ['*.jar'])
}
repositories {
    mavenCentral() //必须
}

// 这里增加了Maven的支持和 uploadArchives 这样一个Task，这个 Task 的作用就是将该 Module 部署到本地的 public 目录下。
uploadArchives {
    repositories {
        mavenDeployer {
            repository(url: uri('../public'))  //注意相对路径，在主项目目录下
            //提交到远程服务器：
            // repository(url: "http://www.xxx.com/repos") {
            //    authentication(userName: "admin", password: "admin")
            // }
            //本地的Maven地址设置为D:/repos
//            repository(url: uri('D:/repos'))
        }
    }
}
```

* 2、在终端中执行 gradle uploadArchives 指令，将插件部署到 public 目录下

    。当插件部署到本地后，就可以在主项目中引用插件了。当插件正式发布后，可以把插件像其它module一样发布到中央库，这样就可以像使用中央库的库项目一样来使用插件了。

* 3、引用插件

    。在 buildSrc 中，系统自动帮开发者自定义的插件提供了引用支持，但自定义 Module 的插件中，开发者就需要自己来添加自定义插件的引用支持。在**主项目**的 build.gradle 文件中，添加如下所示的脚本：

```
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    
    repositories {
        maven {
            url uri('public')
        }
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.4.2'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.1'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'

        // classpath指定的路径，就是类似compile引用的方式：classpath '[groupId]:[artifactId]:[version]' 

        classpath 'com.pepe.plugin:MyGradlePlugin:3.0.0'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        google()
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```
配置完毕后，就可以在app项目中使用自定义的插件了

在app的build.gradle引入
```
//app.gradle
apply plugin: 'mygradleplugin'
```

引入插件之后，执行如下命令可以一遍更新插件，一边实时预览效果

```
gradle uploadArchives && gradle showPersonInfo1
```

* 4、获取android的配置参数

对于插件开发，很多时候需要依赖其他插件，比如android插件开发，为了开发某些功能，我们需要不可避免的获取android gradle的配置参数和api，因此我们需要添加相应的依赖库到 MyGradlePlugin 插件的 build.gradle 中

```
dependencies {
    implementation gradleApi() //必须
    implementation localGroovy() //必须
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.tools.build:gradle:3.4.2'
}
```
插件类apply中，使用如下方式获取

```
project.task('showStandAlonePlugin1', group: 'mygroup') {
            doLast {
                println('task in StandAlonePlugin1')

                def android = project.extensions.getByType(com.android.build.gradle.AppExtension)
                android.applicationVariants.all{variant ->
                    println '::::>>> '+variant.buildType.name
                    println "applicationId: "+android.defaultConfig.applicationId
                }
            }
        }
```
执行结果如下：
```
> Task :app:showStandAlonePlugin1
task in StandAlonePlugin1
::::>>> debug
applicationId: com.a1one.myapplication
::::>>> release
applicationId: com.a1one.myapplication
```




参考：

[一篇文章带你了解Gradle插件的所有创建方式](https://www.jianshu.com/p/5b99e3af4d6b)

[Gradle 实现自定义插件 - 小雨伞漂流记 - OSCHINA](https://my.oschina.net/ososchina/blog/2994131)

[Gradle新建插件buildSrc - 小妖666个人笔记 - CSDN博客](https://blog.csdn.net/weixin_38883338/article/details/90727880)

[在AndroidStudio中自定义Gradle插件 - huachao1001的专栏 - CSDN博客](https://blog.csdn.net/huachao1001/article/details/51810328)

[Gradle自定义插件以及发布方法 - 简书](https://www.jianshu.com/p/d1d7fd48ff0b)

gradle插件上传Jcenter与自建Maven私服 - pf_1308108803的博客 - CSDN博客
https://blog.csdn.net/pf_1308108803/article/details/78119591

 [Gradle入门到实战(一) — 全面了解Gradle](https://mp.weixin.qq.com/s?__biz=Mzg2NzAwMjY4MQ==&mid=2247483789&idx=1&sn=4b3bb2ab721c8ed7e05f1e8b2e0fbf70&chksm=ce4371dbf934f8cd7c484e8c5356d299bbd5d7790ee11bb0da9725068fa8e4b895f87379949f&token=655420148&lang=zh_CN#rd)