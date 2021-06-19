---
title: StoryOfMyMac
date: 2020-12-06 01:35:17
updated: 2020-12-06 01:35:17
categories: 编程
tags: 感悟
excerpt: 冲动消费冲动消费

---

# 恭喜自己喜提人生第一台Mac

没错这里的MAC是苹果笔记本，不是💄。

Macbook Air M1版本 16/512G

价格也是贵的离谱。

但是直接香的不谈：

1. 第一次发现原来电脑真的不需要鼠标。这触摸板加手势加快捷键好用的就离谱。
2. 续航长的超乎我想象。充一次电完全够你早上8点出门学习晚上11点才回来的量。
3. zsh的终端，比windows的命令提示符高到不知道哪里去了
4. safari访问接力以及AirDrop(其实还没用过)，打通手机和电脑的隔阂

也出现了一些问题：

* CLion不支持调试
* IntelliJ卡死三次，可能是在打开IntelliJ时多次休眠导致
* Navicat在打开众多查询之后卡死，猜想会不会是MacOS的内存管理有与众不同的地方
* 访达这玩意确实怪离谱的，我反正觉得非常***不合理***

## 其他体验

在钻研git命令的时候了解到了zsh里可以开启git命令的代码补全，顺藤摸瓜感受到了一下这种使用shell的快感。用上shell才感觉我这个程序猿对味儿了。

brew,npm,hexo一路过来我需要的工具差不多装完了，这篇博客就是从我的Macbook上创作上传的。

下次研究一下Final Cut Pro和Logic Pro。晚安

2020/12/6 2:33

# homebrew安装

homebrew是mac下的包管理器，类似linux下的apt-get。下载和卸载都比较简单，而Windows上的包管理器还在曲折发展中……

不过更新支持m1芯片的homebrew更换了根目录，所以我需要重新安装。

先` brew bundle dump`将已安装的包目录备份下来。

卸载旧版本：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
```

官网上的使用终端一键安装的命令：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

可惜，raw.githubusercontent.com被dns污染了。

我的解决方式是用vpn翻出去用浏览器访问这个所需脚本，然后把内容复制到本地。

```sudo chmod -R 777 文件目录``` 提权之后把上面命令中字符串换成install.sh的本地路径。

> curl命令访问目标地址，直接输出内容，用$(curl xxx)就表示目标文件内容，如果想要表示本地文件内容可以用$(<"/xxx.abc")

安装好之后`brew bundle`恢复安装的包目录。

好起来了🥳。



