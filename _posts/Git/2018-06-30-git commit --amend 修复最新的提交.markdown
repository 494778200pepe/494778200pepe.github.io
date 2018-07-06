---
layout: post
title:  "git commit --amend 修复最新的提交"
date:   2018-06-30 09:43:00 +0800
categories: Git
tags: Git
author: pepe
description: 『 git commit --amend 修复最新的提交 』
---

> 针对前一个`commit`，如果需要修正，可以`先修改并add`，然后执行`git commit --amend`，会生成一条`新的commit`，并删除之前有错误的`commit`。

```
git add 笑声.txt
git commit --amend              // 需要在原有commit信息基础上重新编辑
git commit --amend --no-edit    // 直接按原有commit信息提交

// 提交并填写提交信息
git commit -m "commit message"
```













