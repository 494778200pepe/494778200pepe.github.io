---
layout: post
title:  "封装ConstraintLayout实现不等字数两端对齐"
date:   2019-07-16 16:09:00 +0800
categories: Android
tags: CustomWidget
author: pepe
description: 『 封装ConstraintLayout实现不等字数两端对齐 』
---

> 原理：利用 `ConstraintLayout` 内部，子元素的 `ChainStyle` 属性，动态添加 `TextView` 到 `ConstraintLayout` 里面。

```
public class MyConstraintLayout extends ConstraintLayout {

    private Context mContext;

    public MyConstraintLayout(Context context) {
        super(context);
        mContext = context;
    }

    public MyConstraintLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        mContext = context;
    }

    public MyConstraintLayout(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        mContext = context;
    }

    int[] ids = {R.id.text_view_1, R.id.text_view_2, R.id.text_view_3, R.id.text_view_4, R.id.text_view_5};

    public void setText(String text,int style) {
        int len = text.length();
        for (int i = 0; i < len; i++) {
            TextView textView = new TextView(mContext);
            textView.setId(ids[i]);
            textView.setText(String.valueOf(text.charAt(i)));

            ConstraintLayout.LayoutParams lp = new ConstraintLayout.LayoutParams(
                    0, 0);
            if( i == 0){
                lp.leftToLeft = this.getId();
                lp.rightToLeft = ids[i + 1];
                lp.horizontalChainStyle = style;
            }else if( i == len -1){
                lp.leftToRight = ids[i - 1];
                lp.rightToRight = this.getId();
            }else{
                lp.leftToRight = ids[i - 1];
                lp.rightToLeft = ids[i + 1];
            }
            textView.setLayoutParams(lp);
            this.addView(textView);
        }
        invalidate();
    }
}
```
同时 在 values 文件夹，里面，新增 ids.xml,提供 id 设置。
```
<?xml version="1.0" encoding="utf-8"?>
<resources>

    <item name="text_view_1" type="id"/>
    <item name="text_view_2" type="id"/>
    <item name="text_view_3" type="id"/>
    <item name="text_view_4" type="id"/>
    <item name="text_view_5" type="id"/>

</resources>
```

参考：

[TextView-不等字数两端对齐 - 简书](https://www.jianshu.com/p/d8f50509b1e9)