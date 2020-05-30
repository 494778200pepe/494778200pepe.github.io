---
layout: post
title:  "小程序 - WXSS选择器after,before"
date:   2020-05-31 07:00:00 +0800
categories: Wechat
tags: 小程序
author: pepe
description: 『 WXSS选择器after,before 』
---

![before_after1]({{ site.baseurl }}/assets/images/wechat/before_after1.png)

做了个小小的例子，看看就知道了。（效果如下图）

![before_after2]({{ site.baseurl }}/assets/images/wechat/before_after2.png)

```
//WXML代码    
<view class="container log-list">
        <text id="txt1">文本内容</text>
        <text id="txt2">一个图标</text>
</view>
```

```
//WXSS代码
#txt1:after {  
  content:"（abc）";
  font-size:25rpx;
  color:red 
  }
#txt1:before {  
  content:"前边内容->";
  font-size:25rpx;
  color:blue;
  background-color:yellow 
  }
#txt2:after {  
    content: url(https://mp.weixin.qq.com/debug/wxadoc/gitbook/images/search@1x.png); 
  }
```



参考：

[WXSS选择器::after,::before，您肯定不知道的！（框架细节七）-微信小程序俱乐部 www.wxappclub.com](http://www.wxappclub.com/topic/489)

[微信小程序のwxss选择器 - 冬冬他哥哥 - 博客园](https://www.cnblogs.com/xietianjiao/p/11936876.html)


























