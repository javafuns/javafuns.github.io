---
layout: default
title:  "Git Cookbook"
date:   2018-01-26 14:26:01 +0800
categories: git
tags: git github
---

# 创建版本库
```
git init
```

# 一些概念
## staged
通过多次执行 git add 把文件放入 staging area，相当于缓冲区，等待 git commit 一次性提交。

## HEAD
HEAD 表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，前溯 100 版本就是 HEAD~100。

# 一些不常用的命令
## git status
查看状态

## git diff --cached
查看 staged 文件与 HEAD 文件之间的 diff 

## git reset --hard HEAD~3
最后 3 个 commit 有问题，想要丢弃(包括本地版本库中的 commit 信息以及文件内容以及工作区文件)，版本库及工作区里的文件变为 3 个版本之前的内容。

## git reset --soft HEAD~3
将最后 3 个 commit 记录从本地版本库中去除，但文件处于 staging area 中且文件内容保持不变。修改文件后，可以再次提交到本地版本库。

## git reflog
查看操作历史:
```
bb01a23 HEAD@{1}: reset: moving to HEAD^^
093355b HEAD@{2}: commit: explain 'git reset --soft'
```
与 git log 的区别是，git log 只能查看到版本库中的 commit，那些 git reset 过的 commit 信息是看不到的。

## git remote
管理与本地版本库相关联的远程版本库：
```
git remote -v # 列出当前关联的远程版本库
git remote add origin git@github.com:xxx/xxx.git  # 添加一个远程版本库与本地版本库关联
git remove <name> # 删除与本地版本库相关联的远程版本库
```

## branch 管理
```
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>
推送本地分支至远程版本库：git push origin branch-name
关联本地分支和远程分支：git branch --set-upstream branch-name origin/branch-name
```

## tag 管理
tag 也是版本库里某个 commit 的一个快照。
tag 具有比 commit 更好记更有意义的名字。
```
git tag <tag name> # 给最新的 commit 打上一个 tag
git tag <tag name> <commit id> # 给某个 commit 打上 tag
git tag # 列出所有 tag
git push origin <tag name> # 将本地 tag push 到远程版本库
git push origin --tags # 一次性推送全部尚未推送到远程的本地标签
```
