---
title: linux
date: 2020-09-08 17:02:06
categories: 配置
tags: 折腾
excerpt: "用WSL2安装linux，实现图形化界面以及安装docker。"
---

因为数据库和docker听说都是linux内核，所以我准备弄一个Linux系统玩玩。

虽然已经用VMware装过ubuntu了，但是出于折腾精神以及希望更快更方便的想法，暂时不考虑双系统的话还有一种方法：WSL。

# 安装WSL2

> 现在最新的WSL2是完全的linux内核，而WSL1则是模仿的linux api。按需下载。

首先是更新win10。~~这一步耽搁了我好久~~

WSL2需要1909以及小版本1049以上的系统。docker desktop需要家庭版系统达到2004以上。不妨直接来个最新的系统。

如果系统自带更新太慢，去微软官网下一个win10易升，更新更快更直接。

在应用和功能里打开WSL功能，进微软商店下载Ubuntu。下好直接点进去就可以设置用户名密码然后使用了。

这样方法装的Ubuntu重装系统非常方便。只需要右键Ubuntu应用设置中重置应用即可。

# 安装docker

安好WSL去docker官网下一个docker desktop，一路确定就可以了。docker安好之后可以直接在WSL的Ubuntu中使用。

具体步骤在它的[官方文档](https://docs.docker.com/get-started)中非常详细。

另外这篇[博客](https://blog.csdn.net/hadues/article/details/104961149)从原理到步骤写的都不错。

# 安装图形化界面

主流的linux图形化界面[^1]之中，我在xfce和gnome中纠结了一段时间。

我打算在主力电脑上装gnome，轻薄本上来xfce试试。

[^1]:可以参考https://www.cnblogs.com/chenmingjun/p/8506995.html

