---
layout: post
title:  "git reflog"
date:   2018-06-30 11:04:00 +0800
categories: Git
tags: Git
author: pepe
description: 『 git reflog 』
---

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











