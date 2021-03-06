---
layout: post
title:  "git rebase 撤销之前的提交"
date:   2018-06-30 11:26:00 +0800
categories: Git
tags: Git
author: pepe
description: 『 git rebase 撤销之前的提交 』
---

### **方法一**
```
git rebase -i HEAD^^
// 直接删除你想要撤销的那一行记录
```

### **方法二**
```
git rebase --onto 「HEAD^^」 「HEAD^」 「branch1」
```
上面这行代码的意思是：以「倒数第二个`commit`」为起点（起点不包含在`rebase`序列里哟），「`branch1`」为终点`rebase`到「倒数第三个`commit`」上。


### **在合并分支时，如果不想合并某个commit**
如果要剔除多个`commit`，那么要求剔除的`commit`必须是连续的。

```
git rebase --onto 「master的第3个commit」 「branch1的第4个commit」 「branch1」
```

`--onto`参数后面有三个附加参数：目标`commit`、起点`commit`（注意：`rebase`的时候会把起点排除在外）、终点`commit`。所以上面这行指令就会从「`branch1`的第4个`commit`」 往下数，拿到`branch1`所指向的「第5个`commit`」，然后把 「第5个`commit`」重新提交到「`master`的第3个`commit`」上去。


























