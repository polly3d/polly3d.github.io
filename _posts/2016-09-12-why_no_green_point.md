---
layout: post
title: 提交github后，无绿点图
---

之前一段时间都有提交代码到github，首页的绿点图上一直没有绿一点。仔细查找了一下原因，github上原来是有明确说明的。

# github说明引用

对于提交的commit，github有如下要求：

```
Commits will appear on your contributions graph if they meet all of the following conditions:

The commits were made within the past year.
The email address used for the commits is associated with your GitHub account.
The commits were made in a standalone repository, not a fork.
The commits were made:
In the repository's default branch (usually master)
In the gh-pages branch (for repositories with Project Pages sites)

```

针对这个要求，问题就在于email上。因为本机的email与github不相同。

# 解决办法

在git命令行中，输入如下命令：

```
git config user.email "polly3d@126.com"
```

如果需要，可以加入`--global`参数，使设置全局有效。


