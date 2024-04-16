---
title: Git命令
date: 2023-11-13 22:59:12
categories: [Git]
tags: [Git]
typora-root-url: ..
---

### 提交

```bash
git commit -m "commit_info"
```

### 创建分支

是指在当前节点下，创建一个完全一样的分支，和之前的分支互不干扰。

```bash
git branch <branch_name>
```

切换到相应的分支上：

```bash
git checkout <branch_name>
```

可以用一个指令完成创建与移动：

```bash
git checkout -b <branch_name>
```

<!--more-->

### 合并

例如我有两个分支，在每个分支上都有各自不同的更改，我们需要将两个节点本身及它们所有的祖先都包含进来。

比如，我有两个分支，分别叫做`main`和`bugFix`。我希望把`bugFix`的修改后的内容给合并到我修改后的`main`分支里。

首先切换到相应的分支：

```bash
git checkout main
```

和其他的分支合并：

```bash
git merge bugFix
```

### 切换提交

你可以使用`git checkout `命令切换到特定的提交状态。

这将使你进入"分离头指针"状态，只能查看历史记录，而不能进行分支操作。

我的`HEAD`指针原本指向我最新的提交记录，通过下面的指令将会指向具体的节点：

```bash
git checkout "commit_hash"
```

使用`commit_hash`来移动头指针的话会比较麻烦，毕竟`hash`值很长，因此有了**相对引用**的概念出现：

使用`^`和`~`来进行相对引用：

```bash
git checkout <branch_name>^
```

这里就是指向了相应`branch_name`的`parent`节点。

例如此时我有一个分支叫做`main`：

```bash
commit_hash_before
commit_hash_now <- main
```

在这里`parent`节点指的就是`commit_hash_before`。

还可以进行强制移动：

```bash
git branch -f <branch_name> HEAD~3
git branch -f <branch_name> "commit_hash"
```

这里会把相应`branch_name`的分支移动到头指针的第三级`parent`提交。

### 撤销变更

本地上我们可以用`git reset`来进行变更的撤销：

```bash
git reset "commit_hash"
```

对于多人协作的共同的分支，为了撤销更改并分享给他人，可以使用`git revert`指令。

```bash
git revert "commit_hash"
```

### 远程操作

将远程仓库克隆到本地：

```bash
git clone
```

拉取远程仓库的修改，合并到本地的分支：

```bash
git pull
```

把本地仓库的修改，推送到远程仓库里：

```bash
git push
```

如果远程仓库进行了和我们本地不同的改变，可以使用如下的指令进行拉取，融合和推送：

```bash
git pull --rebase
git push
```