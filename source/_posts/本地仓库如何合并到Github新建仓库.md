---
title: 本地仓库如何合并到Github新建仓库
date: '2024-12-18 18:46:53'
math: false
excerpt: ''
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