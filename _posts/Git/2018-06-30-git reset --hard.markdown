---
layout: post
title:  "git reset --hard"
date:   2018-06-30 11:05:00 +0800
categories: Git
tags: Git
author: pepe
description: 『 git reset --hard 』
---

> `reset`的本质：移动`HEAD`以及它所指向的`branch`。

### **reset --hard：重置工作目录**
`--hard`:暂存区和未tracked的文件会被擦除。

> `reset --hard`会在重置`HEAD`和`branch`的同时，重置工作目录里的内容。当你在`reset`后面加了`--hard`参数时，你的工作目录里的内容会被完全重置为和`HEAD` 的新位置相同的内容。换句话说，就是你的未提交的修改会被全部擦掉。

### **reset --soft：保留工作目录**
`--soft`:暂存区和未tracked的文件，会继续保留。同时当前commit的改动，会放进暂存区。

> `reset --soft`会在重置`HEAD`和`branch`时，保留工作目录和暂存区中的内容，并把重置`HEAD`所带来的新的差异放进暂存区。

### **reset 不加参数：保留工作目录，并清空暂存区**
> 不加参数，就是默认`--mixed`：跟`reset --soft`差不多，不过就是暂存区的内容被移出，挪到非暂存区(`已tracked`)。`untracked`的文件继续`untracked`。同时当前`commit`的改动，会放非暂存区(`untracked`)。



