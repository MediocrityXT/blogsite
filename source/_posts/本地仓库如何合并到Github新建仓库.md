---
title: Github使用小知识
date: 2024-12-18 18:46:53
math: false
excerpt: 本地仓库如何合并到Github新建仓库？已push的上上一个commit名字写错了怎么改？看我吧～
tags:
- linux
- github
- instructions
---

# 本地仓库如何合并到Github新建仓库
1.  【Github】网页选择新建仓库，选择README或者License等会给你新仓库添加这些文件，但不影响。复制仓库git地址 `git@github.com :USERNAME/REPONAME.git`
2. 【本地】`git init && git remote add origin git@github.com :USERNAME/REPONAME.git`
3. 【本地】 `git pull --set-upstream origin main ` 拉取Github仓库中的README或者License，并且设定本地和Github分支同步。
4. 【本地】修改好 `.gitignore` 之后，提交commit并推送仓库` git add . && git commit -m 'first commit' && git push`

# 已push的上上一个commit名字写错了怎么改
1. 彻底丢弃没提交的上一个Commit，也丢弃工作区 `git reset --hard HEAD^`
2. 修改本地的上上一个commit的信息 `git commit --amend`
3. 强制将修改后的commit推送到云端 `git push -f`
# Github跨系统路径名冲突导致无法同步远程分支

在windows上进行git pull，可能会出现error: invalid path的问题，原因是linux/mac允许的路径名可能在windows的NTFS系统里不合法。
> NTFS路径里不能包含特殊字符 `\/<>|":*?`

**临时解决方法，但不是长久之计**
`git config --global core.protectNTFS false`
再pull可以拉下来所有路径合法的文件，但是路径不合法的目录及其中的文件不行。
不存在的路径，还是会影响后续同步远程分支

## Git clone之后无法和远程完全绑定
1. Git clone之后只会有一个本地master/main分支，跟随远程的master/main
2. 如果这时候git checkout dev或者origin/dev。则会把本地的master文件更新成远程的dev分支的文件和commit。但是不会自动跟踪上去，HEAD detached state，HEAD没有指向任何本地branch。（因为还没新建本地branch，而HEAD必须指向本地分支）
3. 如果错误的写一个本地不存在的远程也不存在的分支名，git checkout abc，会报错。
4. **本地必须要新建一个分支dev，和远程dev分支对应**，所以我们必须要用`git checkout -b dev origin/dev`
	1. 如果这里由于跨系统问题无法完全同步，则不会新建本地dev，但是会更新剩下的文件，这时候新建本地分支git checkout -b dev
	2. 然后重置本地 dev 分支以匹配远程 dev 分支（这会丢失本地 dev 分支上的任何未提交或未推送的更改）：git reset --hard origin/dev
	3. 但是如果远程dev分支仍然存在不合法目录和文件，不能完全和最新commit一致，reset依然会失败
5. 最终解决方法：linux上把非法路径全部删掉，更新commit

参考资料：
[gitcheckout命令详解 • Worktile社区](https://worktile.com/kb/ask/250609.html)
[HEAD detached at ----CSDN博客](https://blog.csdn.net/qq_35008279/article/details/97108202)
 [git拉取代码报错invalid path解决，以及windows的一个坑 - CodingLyfe - 博客园](https://www.cnblogs.com/codinglyfe/p/18547894)
# 如何回到branch最初的干净情况 ：
1. 切换回该分支 `git checkout master`
2. 删除未追踪的文件和目录` git clean -fd`
3. 恢复所有删除的更改的文件 `git checkout .`
4. 检查是否干净 `git status`