---
title: Homebrew On Mac
date: '2020-12-06 01:35:17'
math: false
excerpt: ''
tags:
- macos
- instructions
---

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