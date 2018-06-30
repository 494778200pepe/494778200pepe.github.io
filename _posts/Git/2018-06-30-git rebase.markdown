---
layout: post
title:  "git rebase"
date:   2018-06-30 09:31:00 +0800
categories: Git
tags: Git
author: pepe
description: 『 git rebase 』
---

> 有些人不喜欢`merge`，因为在`merge`之后，`commit`历史就会出现分叉，这种分叉再汇合的结构会让有些人觉得混乱而难以管理。如果你不希望`commit`历史出现分叉，可以用`rebase`来代替`merge`。

### **合并branch1到master**
```
git checkout branch1
git rebase master
git checkout master
git merge branch1
```

> `rebase`的原理是：将`branch1`上有的，而`master`上没有的`commit`，复制一份到`master`上。之后原`branch1`上的「`master`上没有的`commit`」，就会因为不在分支上，而废弃消失。


> 不直接在`master`上`rebase`，是因为`rebase`之后，`master`上的`commit`就会消失，而远程仓库上是存在的。















