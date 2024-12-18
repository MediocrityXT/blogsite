---
title: 被WSL2折磨到win本刷Linux
date: 2020-09-08 17:02:06
math: false
excerpt: 用WSL2安装linux，实现图形化界面以及安装docker。
tags:
- linux
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

我原本打算在主力电脑上装gnome，轻薄本上来xfce试试。

结果wsl上怎么装vnc和xlaunch怎么配置都没获取到视频信号输入，气得我^@&)(\$&^@)$)@(&*

---

经历了漫长的痛苦之后，放弃了原想法。现在WSL上准备不做图形化了，WSL定位在于跑一些大型的linux程序。轻薄本上ubuntu desktop，自带gnome图形化界面，轻松许多。

# 轻薄本改造

我决定对我的千元本中柏ezbook2进行改造。原本的win10系统跑起来非常慢，不够流畅；我突发奇想，既然要装linux，为什么不直接在这个本子上装一个呢？

> “查拉图斯特拉的沉沦自此开始”

我用u盘下了ubuntu镜像之后，直接覆盖掉原本的win10，系统看起来运行正常。然后问题来了：

- 找不到wifi适配器。用`lspci`和`ip a`指令都无法看到我的网卡。
- 触摸板没有任何反应。

## 无线网卡

中柏用的到底是什么网卡我到现在仍然不知道。我尝试过以下方法，都失败了。

- 安装系统盘自带的dkms和broadcom驱动
- 用NdisWrapper工具安装新款ezbook2的驱动

然后依然没有找到网卡。

问了淘宝客服，没有给予任何帮助，服务态度贼差。小心中柏！

---

在售后群里下到我这个老款ezbook2的驱动包，发现我的wifi和蓝牙是同一个集成模块：AP6212。

~~人家开发板上用这个小模块。你给我笔记本上装个网卡就这？？~~

继续google，发现AP6212使用的固件是broadcom的43430。

不断谷歌之后采取了如下办法：

1. 重装最新版本ubuntu——从18.04升到20.04（新系统自带许多驱动，装完新系统声卡和触摸板立即能用了）
2. 输入`dmesg | grep "brcm"` 查看linux内核关于brcm的日志。这一步非常重要，提供非常多关键信息！
3. 发现问题在于某些固件并没有加载上。
4. 在看了200+帖子和文章之后，找到一个奇妙的方法[^2]：将/lib/firmware/brcm中43430的已有驱动文件直接复制改名成所需驱动文件(比如`brcmfmac43430a0-sdio.AP6212.txt`改成`brcmfmac43430a0-sdio.txt`) ~~你还别说 真有用 绝了~~
5. 通过上述方法，将wifi和蓝牙所需固件放入brcm文件夹后`reboot`
6. Wi-Fi and bluetooth work well~XD

[^1]: 可以参考https://www.cnblogs.com/chenmingjun/p/8506995.html
[^2]: 感谢ask ubuntu！https://askubuntu.com/questions/1156698/wifi-driver-not-found-in-mini-pc-ubuntu-18-04