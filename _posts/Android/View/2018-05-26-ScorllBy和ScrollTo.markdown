---
layout: post
title:  "ScorllBy和ScrollTo"
date:   2018-05-26 14:29:10 +0800
categories: Android
tags: 自定义View
author: pepe
description: 『 ScorllBy和ScrollTo 』
---

```
public class View {
    ....
    protected int mScrollX;   //该视图内容相当于视图起始坐标的偏移量   ， X轴 方向
    protected int mScrollY;   //该视图内容相当于视图起始坐标的偏移量   ， Y轴方向
    //返回值
    public final int getScrollX() {
        return mScrollX;
    }
    public final int getScrollY() {
        return mScrollY;
    }
    public void scrollTo(int x, int y) {
        //偏移位置发生了改变
        if (mScrollX != x || mScrollY != y) {
            int oldX = mScrollX;
            int oldY = mScrollY;
            mScrollX = x;  //赋新值，保存当前便宜量
            mScrollY = y;
            //回调onScrollChanged方法
            onScrollChanged(mScrollX, mScrollY, oldX, oldY);
            if (!awakenScrollBars()) {
                invalidate();  //一般都引起重绘
            }
        }
    }
    // 看出原因了吧 。。 mScrollX 与 mScrollY 代表我们当前偏移的位置 ， 在当前位置继续偏移(x ,y)个单位
    public void scrollBy(int x, int y) {
        scrollTo(mScrollX + x, mScrollY + y);
    }
    //...
}
```
说明：

* 1、`scrollTo` 和 `scrollBy` 只能改变 『View内容』 的位置，而不能改变View在布局中的位置。
* 2、在滑动过程中，mScrollX 的值总等于 『View左边缘』 和 『View内容左边缘』 在水平方向上的距离。
* 3、mScrollY 的值总等于 『View上边缘』 和 『View内容上边缘』 在垂直方向上的距离。
* 4、mScrollX 和 mScrollY 的单位为像素。
* 5、如果`从左向右`滑动，那么 `mScrollX 为负值`，反之为正值。
* 6、如果`从上往下`滑动，那么 `mScrollY 为负值`，反之为正值。

































