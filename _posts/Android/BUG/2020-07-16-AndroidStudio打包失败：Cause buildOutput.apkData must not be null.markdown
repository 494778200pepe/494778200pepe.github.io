---
layout: post
title:  "AS 出现 item inner element must either be a resource reference or empty"
date:   2020-07-16 16:55:00 +0800
categories: Android
tags: BUG
author: pepe
description: 『 AS 出现 item inner element must either be a resource reference or empty 』
---


今天更新AS之后，之前有一个老的项目出现如上所示的问题，导致build失败。查看错误问题之后发现代码是如下格式的：

```
<item name="animator" type="id">false</item>
<item name="date_picker_day" type="id">false</item>
```

然后网上找到了一些资料：

在开发文档中，https://developer.android.com/guide/topics/resources/more-resources#Id
也有提到说上面那种方式已经不能这么写了，新的方法如下：

```
<item name="animator" type="id"/>
<item name="date_picker_day" type="id"/>
```

如果引入的是第三方包怎么修改呢？
1、在你的项目res\values文件夹下新建`ids.xml`文件

2、将之前编译出错的item重新写一遍，改成下面的

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <item name="animator" type="id"/>
    <item name="date_picker_day" type="id"/>
   ...
</resources>
```

重新build之后AS会自动处理这些问题。

参考：

[AS 出现:<item> inner element must either be a resource reference or empty. - 简书](https://www.jianshu.com/p/592308a335ff?tdsourcetag=s_pcqq_aiomsg)





