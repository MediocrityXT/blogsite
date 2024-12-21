---
title: Windows自带Webdav服务器
date: '2024-12-21 19:41:46'
math: false
excerpt: ''
tags:
- windows
- instructions
- server
- macos
---

Internet Information Services, IIS 是一个Windows自带的可选安装程序。
安装方法：[搭建webDAV（免费低配nas/内网视频资源中心）\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1rD4y1g7Mv)
按教程安装之后可以启动一个网站，这个网站可以将本地一个共享文件夹作为根目录，然后支持远程webdav方式访问。

注意
1. 服务器IP如果变了，这个IIS的网站需要重新绑定IP地址。默认webdav的端口是8090。
2. Guest用户可以设置密码为空。

# macos在网络路径里到处生成.DS_Store
Mac电脑复制文件到Window系统会有很多 `._` 或者 `.DS_Store` 文件。 

阻止macos在外部路径生成.DS_Store，终端命令输入后重启电脑
 `defaults write com.apple.desktopservices DSDontWriteNetworkStores true`

Windows删除这些文件：
`rm ._*`


# 参考文献
[永久关闭Mac自动生成.ds_store文件-CSDN博客](https://blog.csdn.net/weixin_35752122/article/details/129596540?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-1-129596540-blog-135481990.235^v43^pc_blog_bottom_relevance_base6&spm=1001.2101.3001.4242.2&utm_relevant_index=4)

[Mac电脑复制文件到Window系统会有很多"._"或者".DS_Store"开头的文件 - ^Mao^ - 博客园](https://www.cnblogs.com/it774274680/p/17444700.html#%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95)