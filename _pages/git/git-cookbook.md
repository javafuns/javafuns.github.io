# 创建版本库
```
git init
```

# 一些概念
## staged
通过多次执行 git add 把文件放入 staging area，相当于缓冲区，等待 git commit 一次性提交。

## HEAD
HEAD 表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，前溯 100 版本就是 HEAD~100。

# 一些不常用的命令
## git status
查看状态

## git diff --cached
查看 staged 文件与 HEAD 文件之间的 diff 

## git reset --hard HEAD~3
最后 3 个 commit 有问题，想要丢弃(包括本地版本库中的 commit 信息以及文件内容以及工作区文件)，版本库及工作区里的文件变为 3 个版本之前的内容。

## git reset --soft HEAD~3
将最后 3 个 commit 记录从本地版本库中去除，但文件处于 staging area 中且文件内容保持不变。修改文件后，可以再次提交到本地版本库。