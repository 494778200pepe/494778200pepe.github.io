---
layout: post
title:  "ViewTreeObserver简介"
date:   2018-02-02 13:10:00 +0800
categories: Android
tags: Android
author: pepe
description: Android ViewTreeObserver简介.
---

### 一、结构
~~~
public final class ViewTreeObserver extends Object
    java.lang.Object
         android.view.ViewTreeObserver
~~~ 
### 二、概述
　　　　
* 这是一个注册监听视图树的观察者(observer)，在视图树种全局事件改变时得到通知。
* 这个全局事件不仅还包括整个树的布局，从绘画过程开始，触摸模式的改变等。
* `ViewTreeObserver`不能够被应用程序实例化，因为它是由视图提供，参照getViewTreeObserver()以查看更多信息。
 
### 三、内部类　　
~~~
    interface  ViewTreeObserver.OnGlobalFocusChangeListener         
　　//当在一个视图树中的焦点状态发生改变时，所要调用的回调函数的接口类
 
　　interface  ViewTreeObserver.OnGlobalLayoutListener
　　//当在一个视图树中全局布局发生改变或者视图树中的某个视图的可视状态发生改变时，所要调用的回调函数的接口类
 
　　interface  ViewTreeObserver.OnPreDrawListener
　　//当一个视图树将要绘制时，所要调用的回调函数的接口类
 
　　interface  ViewTreeObserver.OnScrollChangedListener
　　//当一个视图树中的一些组件发生滚动时，所要调用的回调函数的接口类
 
　　interface  ViewTreeObserver.OnTouchModeChangeListener
　　//当一个视图树的触摸模式发生改变时，所要调用的回调函数的接口类
~~~
 
四、公共方法
~~~　　
  /**注册一个回调函数，当在一个视图树中的焦点状态发生改变时调用这个回调函数。
　 * 参数 listener    将要被添加的回调函数
　 *异常 IllegalStateException       如果isAlive() 返回false
　 */
　public void addOnGlobalFocusChangeListener (ViewTreeObserver.OnGlobalFocusChangeListener listener)
     
 
　/**注册一个回调函数，当在一个视图树中全局布局发生改变或者视图树中的某个视图的可视状态发生改变时调用这个回调函数。
　 *参数 listener    将要被添加的回调函数
　 *异常 IllegalStateException       如果isAlive() 返回false
　 */
　public void addOnGlobalLayoutListener (ViewTreeObserver.OnGlobalLayoutListener listener)
　　
 
　　
　/**注册一个回调函数，当一个视图树将要绘制时调用这个回调函数。
　 *参数 listener    将要被添加的回调函数
　 *异常 IllegalStateException       如果isAlive() 返回false
　　*/
　public void addOnPreDrawListener (ViewTreeObserver.OnPreDrawListener listener)
 
　   
　/**注册一个回调函数，当一个视图发生滚动时调用这个回调函数。
　 *参数 listener    将要被添加的回调函数
　 *异常 IllegalStateException       如果isAlive() 返回false
　　*/
　public void addOnScrollChangedListener (ViewTreeObserver.OnScrollChangedListener listener)  
 
　
　/**注册一个回调函数，当一个触摸模式发生改变时调用这个回调函数。
　 *参数 listener    将要被添加的回调函数
　 *异常 IllegalStateException       如果isAlive() 返回false
　 */
　public void addOnTouchModeChangeListener (ViewTreeObserver.OnTouchModeChangeListener listener)
 
　　
　//当整个布局发生改变时通知相应的注册监听器。如果你强制对视图布局或者在一个没有附加到一个窗口的视图的层次结构或者在GONE状态下，它可以被手动的调用
　public final void dispatchOnGlobalLayout ()
    
　/**当一个视图树将要绘制时通知相应的注册监听器。如果这个监听器返回true，则这个绘制将被取消并重新计划。如果你强制对视图布局或者在一个没有附加到一个窗口的视图的层次结构或者在一个GONE状态下，它可以被手动的调用
　 *返回值  当前绘制能够取消并重新计划则返回true，否则返回false。
　 */
　public final boolean dispatchOnPreDraw ()
 
　/**指示当前的ViewTreeObserver是否可用(alive)。当observer不可用时，任何方法的调用（除了这个方法）都将抛出一个异常。如果一个应用程序保持和ViewTreeObserver一个历时较长的引用，它应该总是需要在调用别的方法之前去检测这个方法的返回值。
　　*返回值 但这个对象可用则返回true，否则返回false   
　 */
　public boolean isAlive ()
    
     
　/**移除之前已经注册的全局布局回调函数。
　 *参数 victim 将要被移除的回调函数
　 *异常 IllegalStateException       如果isAlive() 返回false   
　 */
　public void removeGlobalOnLayoutListener (ViewTreeObserver.OnGlobalLayoutListener victim)
　　
　/**移除之前已经注册的焦点改变回调函数。
　 *参数 victim 将要被移除的回调函数
　　*异常 IllegalStateException       如果isAlive() 返回false 
　 */
　public void removeOnGlobalFocusChangeListener (ViewTreeObserver.OnGlobalFocusChangeListener victim)
　　
　/**移除之前已经注册的预绘制回调函数。
　　*参数 victim 将要被移除的回调函数
　 *异常 IllegalStateException       如果isAlive() 返回false  
　 */
　public void removeOnPreDrawListener (ViewTreeObserver.OnPreDrawListener victim)
　　
　/**移除之前已经注册的滚动改变回调函数。
　 *参数 victim 将要被移除的回调函数
　　*异常 IllegalStateException       如果isAlive() 返回false 
　　*/
　public void removeOnScrollChangedListener (ViewTreeObserver.OnScrollChangedListener victim)
　
　/**移除之前已经注册的触摸模式改变回调函数
　　*参数 victim 将要被移除的回调函数
　　*异常 　IllegalStateException       如果isAlive() 返回false
　 */
　public void removeOnTouchModeChangeListener (ViewTreeObserver.OnTouchModeChangeListener victim)
~~~

参考：

[ViewTreeObserver简介 - zhangze - 博客园](http://www.cnblogs.com/xingfuzzhd/p/3154674.html)

