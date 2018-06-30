---
layout: post
title:  "git rebase -i 修复之前的提交"
date:   2018-06-30 10:21:00 +0800
categories: Git
tags: Git
author: pepe
description: 『 git rebase -i 修复之前的提交 』
---

> 如果不是最新的`commit`写错，就不能用`commit --amend`来修复了，而是要用`rebase`。不过需要给`rebase`也加一个参数：`-i`。

```
rebase -i 是 rebase --interactive 的缩写形式，意为「交互式 rebase」。
```

所谓「交互式 rebase」，就是在`rebase`的操作执行之前，你可以指定要`rebase`的`commit`链中的每一个`commit`是否需要进一步修改。

### **开启交互式`rebase`过程**
```
git rebase -i HEAD^^
```

说明：在 Git 中，有两个「偏移符号」：` ^ 和 ~`。 

> `^`的用法：在`commit`的后面加一个或多个`^`号，可以把`commit`往回偏移，偏移的数量是`^`的数量。例如：`master^`表示`master`指向的`commit`之前的那个`commit； HEAD^^`表示`HEAD`所指向的`commit`往前数两个`commit`。

> `~`的用法：在`commit`的后面加上`~`号和一个数，可以把`commit`往回偏移，偏移的数量是`~`号后面的数。例如：`HEAD~5`表示`HEAD`指向的`commit`往前数 5 个`commit`。














