---
title: Github使用小知识
date: 2024-12-18 18:46:53
math: false
excerpt: 本地仓库如何合并到Github新建仓库？已push的上上一个commit名字写错了怎么改
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