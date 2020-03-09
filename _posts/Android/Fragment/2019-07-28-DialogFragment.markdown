---
layout: post
title:  "DialogFragment"
date:   2019-07-28 19:58:00 +0800
categories: Android
tags: Fragment
author: pepe
description: 『 DialogFragment 』
---

### **DialogFragment的基本使用**

#### 重写onCreateView创建Dialog

```
	@Override
	public View onCreateView(LayoutInflater inflater, ViewGroup container,
			Bundle savedInstanceState){
		getDialog().requestWindowFeature(Window.FEATURE_NO_TITLE);
		View view = inflater.inflate(R.layout.fragment_edit_name, container);
		return view;
	}
```

#### 重写onCreateDialog创建Dialog
```
	@Override
	public Dialog onCreateDialog(Bundle savedInstanceState)
	{
		AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
		// Get the layout inflater
		LayoutInflater inflater = getActivity().getLayoutInflater();
		View view = inflater.inflate(R.layout.fragment_login_dialog, null);
		// Inflate and set the layout for the dialog
		// Pass null as the parent view because its going in the dialog layout
		builder.setView(view)
				// Add action buttons
				.setPositiveButton("Sign in",
						new DialogInterface.OnClickListener()
						{
							@Override
							public void onClick(DialogInterface dialog, int id)
							{
							}
						}).setNegativeButton("Cancel", null);
		return builder.create();
	}
```


参考：

[Android DialogFragment 使用 - 简书](https://www.jianshu.com/p/d1852b04a0aa)

[Android 官方推荐 : DialogFragment 创建对话框 - Hongyang - CSDN博客](https://blog.csdn.net/lmj623565791/article/details/37815413)


















