---
title: git 小知识记录
---

最近用 git 时遇到几次修改文件名大小写不生效的问题，很是恶心，突发奇想，用 blog 记录一下那些碰到的问题<!-- more -->

## 在 git 中修改文件名大小写

1. 如果是修改文件名，可以用 git mv

```
git mv -f yOuRfIlEnAmE yourfilename
```

2. 修改 gitconfig

```
git config core.ignorecase false


<!-- in ~/.gitconfig -->
[core]
	ignorecase = false
```

3. 删除 git 文件

```
git rm --cached xxxx
```
