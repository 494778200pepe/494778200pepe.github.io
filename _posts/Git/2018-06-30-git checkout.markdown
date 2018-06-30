---
layout: post
title:  "git checkout"
date:   2018-06-30 13:45:00 +0800
categories: Git
tags: Git
author: pepe
description: 『 git checkout 』
---

> `checkout`的本质是签出指定的`commit`，所以你不止可以切换`branch`，也可以直接指定`commit`作为参数，来把`HEAD`移动到指定的`commit`。

```
git checkout HEAD^^
git checkout master~5
git checkout 78a4bc
git checkout 78a4bc^
```

### **heckout 和 reset 的不同**

> `checkout`和`reset`都可以切换`HEAD`的位置，它们除了有许多细节的差异外，最大的区别在于：`reset`在移动`HEAD`时会带着它所指向的`branch`一起移动，而`checkout`不会。当你用`checkout`指向其他地方的时候，`HEAD`和 它所指向的`branch`就自动脱离了。

