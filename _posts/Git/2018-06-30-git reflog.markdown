---
layout: post
title:  "git reflog"
date:   2018-06-30 11:04:00 +0800
categories: Git
tags: Git
author: pepe
description: 『 git reflog 』
---

### **reflog：引用的log**
默认查看的是`HEAD`的移动历史。

> 如果在回退以后又想再次回到之前的版本，`git reflog` 可以查看所有分支的所有操作记录（包括`commit`和`reset`的操作），包括已经被删除的`commit`记录，`git log`则不能察看已经删除了的`commit`记录.

```
Administrator@USER-20171026MG MINGW64 ~/Desktop/lyf (master)
$ git reflog
e1bdff6 (HEAD -> master) HEAD@{0}: commit: 第二次提交
62e6739 HEAD@{1}: reset: moving to HEAD^
8113f0d HEAD@{2}: reset: moving to HEAD^
dc6bb4e HEAD@{3}: reset: moving to dc6bb4e
8113f0d HEAD@{4}: reset: moving to HEAD^
dc6bb4e HEAD@{5}: commit: my.txt增加44444内容
8113f0d HEAD@{6}: commit: 文件增加33333内容
62e6739 HEAD@{7}: commit (initial): my第一次提交
```


### **找回branch**

如果你删除了某个branch，后面又想找回：
```
git reflog
// 找到目标branch删除之前，最后一次移动到该branch的记录
git checkout c08de9a「记录」
git checkout -b branch1
```
> 注意：不再被引用直接或间接指向的`commits`会在一定时间后被 Git 回收，所以使用`reflog`来找回删除的`branch`的操作一定要及时，不然有可能会由于`commit`被回收而再也找不回来。

### **查看其他引用的 reflog**

`reflog`默认查看`HEAD`的移动历史，除此之外，也可以手动加上名称来查看其他引用的移动历史，例如某个`branch`：
```
git reflog master
```





