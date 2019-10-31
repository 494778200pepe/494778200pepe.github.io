---
layout: post
title:  "AppBarLayout"
date:   2019-10-30 16:46:00 +0800
categories: Android
tags: MD
author: pepe
description: 『 AppBarLayout 』
---

AppBarLayout继承自LinearLayout，布局方向为垂直方向。但是它内部封装了一些手势变换的动画。
首先它需要依赖CoordinatorLayout作为父容器，同时也要求一个具有可以独立滚动的子View。

> AppBarLayout通过设置layout_scrollFlags参数，来控制AppBarLayout中控件的行为.

* childView : 就是AppBarLayout内的某个孩子控件;

* ScrollingView : 就是可以滚动的 View;比如 RecyclerView（实现了 NestedScrollingChild2），NestedScrollView 等等，

* Your scrollable view 只支持实现了 NestedScrollingChild 的View

* 所以 ScrollingView，可以是 内部嵌套有 RecyclerView 的 NestedScrollView，也可以是单独的一个 RecyclerView

```
通过在scrollable view上面设置了 app:layout_behavior="@string/appbar_scrolling_view_behavior" ，

当它滚动的时候，AppBarLayout会回调触发内部设置了 app:layout_scrollFlags=""的控件的滚动行为	
```

> 其主要功能是可以让其子View可以响应对位于与 AppBarLayout 同一层级的某个可滚动View（可理解为 ScrollView）的滚动事件
（也就是说，当与 AppBarLayout 同一层级的某个可滚动View发生滚动时，
你可以定制让 AppBarLayout 的子View响应这些滚动事件（比如让子View发生滚动，或者保持不动等等）。

AppBarLayout 如何与兄弟节点（ScrollView）进行绑定，进而接收到其滚动事件：
让我们再拆分一下这个逻辑，其实就是 AppBarLayout 要与同一层级的ScrollView进行交互。

* CoordinatorLayout 的作用就是提供子View之间的交互。

* 因此，通过使用 CoordinatorLayout 包裹 AppBarLayout 和 ScrollView，并提供适当的 Behavior，就可以完成这两者的交互了。

* 而这个 Behavior 就是 AppBarLayout.ScrollingViewBehavior，

* 我们可以直接为ScrollView绑定这个 AppBarLayout.ScrollingViewBehavior

* （绑定的方法可以通过配置xml文件：app:layout_behavior="@string/appbar_scrolling_view_behavior"，

* 这个 Google 为我们提供的appbar_scrolling_view_behavior,

* 其实就是 AppBarLayout.ScrollingViewBehavior 的类名：android.support.design.widget.AppBarLayout$ScrollingViewBehavior），

* 这样，AppBarLayout 就能接收到ScrollView的滚动事件了。


AppBarLayout可以为控件设置5种scrollFlag行为：`scroll`, `enterAlways`, `enterAlwaysCollapsed`, `exitUntilCollapsed`, `snap`
当然这5中行为并不是单独使用，有的需要相互结合才会有效果。

### **scroll**

Child View 伴随着scrollingView的滚动事件而滚出或滚进屏幕:

使用这个要注意两点：

* 第一点:如果使用了其他值，必定要使用这个值才能起作用；

* 第二点：如果在这个child View前面的任何其他Child View没有设置这个值，那么这个Child View的设置将失去作用。

使用说明：

* 当 ScrollView 将要向下滚动的时候，优先滚动的是 ScrollView，当 ScrollView 滚动到顶部出现的时候，再开始触发滚动AppBarLayoout中的childView；

* 当 ScrollView 将要向上滚动的时候， 优先将 AppBarLayout的childView 滚出屏幕，然后 ScrollView 才开始滚动；

![scrollFlag1](https://media.giphy.com/media/LPfZ3lwXBHFXkgFcfw/giphy.gif)

![scrollFlag1]({{ site.baseurl }}/assets/images/android/md/scrollFlag1.png)

### **scroll|enterAlways**

使用enterAlways，必须要带上scroll,否则没有效果，同样使用后面哪一个都要有scroll;使用要两个一块使用；

enterAlways决定向下滚动时Scrolling View和Child View之间的滚动优先级问题。

使用说明：

* 当ScrollView将要向下滚动的时候，优先滚动的是AppBarLayout中的childView，当childView完全滚动进入屏幕的时候，才开始滚动 ScrollView；

* 当ScrollView将要向上滚动的时候， 优先将AppBarLayout的childView滚出屏幕，然后ScrollView才开始滚动；

![scrollFlag2](https://media.giphy.com/media/iIFc0atoVyA1pLJUSp/giphy.gif)

![scrollFlag2]({{ site.baseurl }}/assets/images/android/md/scrollFlag2.png)

### **scroll|enterAlways|enterAlwaysCollapsed**

enterAlwaysCollapsed:它是enterAlways的附加值。要使用它，必须三者一起使用；

这里涉及到Child View的高度和最小高度，向下滚动时，

Child View先向下滚动最小高度值，

然后Scrolling View开始滚动，

到达边界时，Child View再向下滚动，直至显示完全。

使用说明：

* 当ScrollView将要向下滚动的时候，优先将AppBarLayout中的childView滚动到它的最小高度，滚动完成之后scrollview才开始自身的滚动，当scrollview滚动完成时，这个时候childView才会将自己高度完全滚动进入屏幕；

* 当ScrollView将要向上滚动的时候， 优先将AppBarLayout的childView滚出屏幕，然后ScrollView才开始滚动；

这里小结一下:使用上面三种组合，当ScrollView要向上滚动的时候没有任何区别；区别只是在于scrollview要向下滚动的时候；

![scrollFlag3](https://media.giphy.com/media/KbM3AhYnxQyzNvZQBE/giphy.gif)
![scrollFlag4](https://media.giphy.com/media/Rhjxns5Da1cM9EeFix/giphy.gif)

![scrollFlag3]({{ site.baseurl }}/assets/images/android/md/scrollFlag3.png)
![scrollFlag4]({{ site.baseurl }}/assets/images/android/md/scrollFlag4.png)

### **scroll|exitUntilCollapsed**
要是exitUntilCollapsed就得这样用才有效果；

这里也涉及到最小高度。发生向上滚动事件时，

Child View向上滚动退出直至最小高度，

然后Scrolling View开始滚动。也就是，Child View不会完全退出屏幕。

使用说明：

* 当ScrollView将要向下滚动的时候，优先滚动的是自己，当自己滚动到顶部头的时候，再开始触发滚动AppBarLayoout中的childView；
这和单纯使用scroll的效果是一致的；

* 当Scrollview将要向上滚动的时候，优先将AppBarLayout中的childView滚动至最小高度，然后scrollview才开始滚动。

![scrollFlag5](https://media.giphy.com/media/JPbTLJQYfWiD3lI4cy/giphy.gif)
![scrollFlag6](https://media.giphy.com/media/h7jgd0dH8PJQDYp5ii/giphy.gif)

![scrollFlag5]({{ site.baseurl }}/assets/images/android/md/scrollFlag5.png)
![scrollFlag6]({{ site.baseurl }}/assets/images/android/md/scrollFlag6.png)

### **scroll|snap**
snap简单理解，就是Child View滚动比例的一个吸附效果。

也就是说，Child View不会存在局部显示,只滚动Child View的部分的情况；

当我们松开手指时，Child View要么向上全部滚出屏幕，要么向下全部滚进屏幕，

有点类似ViewPager的左右滑动

snap 可以和上面任意一个组合使用，使用它可以确保childView不会滑动停止在中间的状态，

当我们松开手指的时候，要么完全显示，要么完全隐藏;

![scrollFlag7](https://media.giphy.com/media/dXiNs2q5sbtluy5fWh/giphy.gif)

![scrollFlag7]({{ site.baseurl }}/assets/images/android/md/scrollFlag7.png)


参考：

[Material Design系列教程（6） - AppBarLayout - 简书](https://www.jianshu.com/p/5e48fb725e3f)

[AppBarLayout中的五种ScrollFlags使用方式汇总 - Android开发积累 - CSDN博客](https://blog.csdn.net/eyishion/article/details/80282204)

[Android 详细分析AppBarLayout的五种ScrollFlags - 简书](https://www.jianshu.com/p/7caa5f4f49bd)

[Gif上传网站](https://giphy.com)



















