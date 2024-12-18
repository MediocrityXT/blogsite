---
title: Obsidian使用
date: '2024-10-10 20:03:36'
math: false
excerpt: ''
tags:
- input
- permanent
- instructions
- obsidian
---

# 源码模式
纯阅读模式无法编辑，点击右下角书本则切换到编辑模式。
注意编辑模式有两种，可以切换，设置为command e切换
1. 源码模式，纯markdown。
2. 实时阅览，所见即所得，类似typora。但是处理不好表格块的多余空行

# 常用快捷键
shift command C 插入代码块
shift command B 插入表格
shift command D 生成当天的笔记
shift command X 标注Callout
shift command Z 添加删除线
`---` 回车生成分隔线

# 高亮块Callout
[Callouts - Obsidian Help](https://help.obsidian.md/Editing+and+formatting/Callouts)
输入 `> [!INFO]` 然后继续输入，两次回车可以生成以下高亮块。大小写不敏感。

> [!tip]
> 默认有12种风格。每一种有不同的颜色和图标。只要把上面例子里的 `INFO` 替换为下面任意一个就行。
> 并且可以在[!tip] 后面加上减号来折叠高亮块内容
- note
- abstract, summary, tldr
- info, todo
- tip, hint, important
- success, check, done
- question, help, faq
- warning, caution, attention
- failure, fail, missing
- danger, error
- bug
- example
- quote, cite

# 双链
obsidian最方便的是 `[[]]` 创建双链，直接跳转其他笔记
如果希望跳转本文的标题，使用 `[[#]]`，例如[卡片学习法](#卡片学习法)
如果希望跳转本文的某个文本块，使用 `[[^]]`
# Front Matter (YAML)
Front Matter里面可以保存很多跟文章本身有关的，但与内容无关的信息，比如[这篇文章中的](https://publish.obsidian.md/chinesehelp/01+2021%E6%96%B0%E6%95%99%E7%A8%8B/Obsidian+%E7%9A%84+YAML+Front+matter+%E4%BB%8B%E7%BB%8D+by+Bon)
```
title: 实际文件名，因为我的文件名可能会是数字串
date: 建立日期
tags:
  - 标签1
alias: 别名1
category: 类别
stars: 星级（可选，当为评测笔记的时候会存在）
from: 基于什么，来自什么
url: 文章链接（如果已经发文或者已经存在在 Obsidian publish 上的话）
```
Obsidian只支持[alias](https://publish.obsidian.md/chinesehelp/03+%E6%95%99%E7%A8%8B/%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8%E5%88%AB%E5%90%8D%EF%BC%88aliases%EF%BC%89)和tags。
alias是文章标题的其他形式，用于双链链接。
如果需要用Alias统一多个不同标题的笔记可以参考[这篇](https://forum-zh.obsidian.md/t/topic/1610/12)
> (双链可以用 `[[原标题|显示标题]]` 的方法解决原标题过长的问题)

之前Hexo里面写博客，Hexo框架编译网站需要每篇md博文都有Front Matter，如下
```
---
title: CV知识
date: 2021-06-05 08:54:48
updated: 2021-06-05 08:54:48
categories: 知识
tags: 笔记
excerpt: YOLO笔记/MobileNet/pytorch CUDA踩坑
Math: true
---
```

| 字段         | 意义               | 必须     |
| ---------- | ---------------- | ------ |
| title      | 博文标题             | 1      |
| date       | 创建时间             | 1      |
| updated    | 修改时间（用于按时间排序文章）  |        |
| categories | 博文类别             | 1      |
| tags       | 博文涉及内容tags       | 1      |
| excerpt    | 手动写的摘要，默认是文章前N个字 |        |
| Math       | 是否需要解析显示数学公式     |        |

对于obsidian日记模版来说，需要保存date, tags

# 第三方插件
[2022年7月，obsidian 依然必装的 10 个插件 | 黑咖啡☕️加奶茶🧋](https://oldwinter.zhubai.love/posts/2162924975395540992)
1. Auto Link Title（自动获取 URL 的网站名字）
2. Advanced Tables（增加 Excel 式的快捷键）
3. Calendar, Periodic Notes (周报)
4. Templater, QuickAdd [【效率办公】Obsidain插件之QuickAdd-强大的快速记录与宏工具 - 哔哩哔哩](https://www.bilibili.com/read/cv14681298/), 
	1. 在 QuickAdd 里面添加 InputAndThinking 这个 Template 类型的 Choice，闪电图标勾选上之后可以在命令列表里找到这个 Template Choice
	2. 设置快捷键 option+1 ，则可以调用 InputAndThinking 模版，然后直接输入文件名“马斯克访谈”即可生成马斯克访谈这个文件。而且里面 title 可以用 {{NAME}} 来直接写入
5. Outliner 用快捷键调整 bullet point item 缩进 (Tab, ShiftTab) 和顺序 (Command+Shift+上下箭头)
6. Linter, EasyTyping 输入增强，比如按一下左引号出两个匹配的引号等等；自动首字母大写
7. Excalidraw. 作为一个原本成熟度就很高的白板软件，【Excalidraw】插件可谓和 Obsidian 完美融合，在实现图片的编辑的同时，还可以直接嵌入笔记显示，甚至在图片中添加反向链接，建立强关联。手绘的风格也是随意、清新，让人一见倾心。

> 手绘流程图，网页版： [Excalidraw | Hand-drawn look & feel • Collaborative • Secure](https://excalidraw.com/)
>  [5 分钟了解 Excalidraw](https://zhuanlan.zhihu.com/p/539137319)
>  [Obsidian 插件：Excalidraw 完美的绘图工具 - 哔哩哔哩](https://www.bilibili.com/read/cv35910019/)
>  [微信公众平台](https://mp.weixin.qq.com/s?__biz=MzU3OTM4OTkzNQ==&mid=2247486976&idx=1&sn=d56ea980aaa27c818736ed9dfcc51332&chksm=fd679e80ca1017960a87ea07b1cf1cfecfdd283d8798a67064f48ea3b2b134e324e707afd185&scene=21#wechat_redirect)
>  Excalidraw 能在 IPAD 上手绘吗？需要去尝试
# 截图如何存到Asset文件夹，并且重命名
[Obsidian插入图片的格式](https://publish.obsidian.md/chinesehelp/03+%E6%95%99%E7%A8%8B/%E5%A6%82%E4%BD%95%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87)
1. ob默认以 `![[]]` 的形式插入图片，并在当前页面中显示。不过自动格式化会将其转化为下面通用MD格式。
2. 通用 MD 格式 `![图片替代文字](图片地址)` 语法也可以解析图片
	1. 图片地址为相对路径
	2. 图片地址只包含名字，则优先在当前目录 `attachments` ，其次根目录 `attachments` 里寻找该名字的图片
3. **注意图片地址里不能包含#**，地址里的空格要改成`%20`

[更改格式设置为 2](https://blog.csdn.net/weixin_41163483/article/details/141834433)，自动将粘贴来的图片放到 attachments 下面。

![](/img/Obsidian使用/Pasted%20image%2020241011165914.png)

 > HEXO 里面是放到和 MD 同名的文件夹中

# Obsidian图片URL识别逻辑

相对路径和绝对路径实验：

实验：vault/奇思妙想/安娜卡列尼娜.md，
有两张同名但不同的图片AB，A:vault/test/image.png, B:vault/CS科技/test/image.png
```
url: ![](test/安娜卡列尼娜-Sonnet.jpg)获得A。基于vault根目录
url: ![](/test/安娜卡列尼娜-Sonnet.jpg)获得A。基于vault根目录
url: ![](./test/安娜卡列尼娜-Sonnet.jpg)无结果
url: ![](../test/安娜卡列尼娜-Sonnet.jpg)获得A。基于md目录
url: ![](CS科技/test/安娜卡列尼娜-Sonnet.jpg)获得B。基于vault根目录
```
删去A图片，再次实验
```
url: ![](test/安娜卡列尼娜-Sonnet.jpg)获得B。自动进入CS科技路径了
url: ![](/test/安娜卡列尼娜-Sonnet.jpg)无结果
url: ![](./test/安娜卡列尼娜-Sonnet.jpg)无结果
url: ![](../test/安娜卡列尼娜-Sonnet.jpg)获得B。自动进入CS科技路径了
url: ![](CS科技/test/安娜卡列尼娜-Sonnet.jpg)获得B。基于vault根目录
```

补充实验1：
图片在vault/attachments/最优化/image-xxxx.png，
对于CS科技/最优化导论.md，我们发现即便只有文件名 `![](image-xxxx.png)` ，obsidian也可以准确识别

补充实验2:
对于CS科技/最优化/最优化导论.md，我们发现即便
1. 只有文件名 `![](image-xxxx.png)` ，可以识别
2.  `![](./image-xxxx.png)` ，识别不了
3.  `![](../image-xxxx.png)` ，识别不了
4. `![](../../image-xxxx.png)` ，可以识别

结论：
1. 绝对路径： `/test/安娜卡列尼娜-Sonnet.jpg` 只会在vault根目录添加该绝对路径，找不到就找不到
2. 相对路径：
	1. 如果相对路径不含任何 `./`，`../`，默认从vault根目录开始算，**在Vault中全局搜索图片**，找到**包含该相对路径**的图片路径
	2. 如果包含任何 `./`，`../`，则从md目录算完所有任何 `./`，`../` 之后，得到临时目录，以及余下相对路径
		1. 如果临时目录是vault根目录，则会**在Vault中全局搜索图片**，找到**包含余下相对路径**的图片路径
		2. 临时目录如果不是根目录，则只在该临时目录添加余下路径，寻找图片
3. 一般情况下obsidian自动refactor会添加的是正确的img到md文件的相对路径
4. 所以我们hexo可能要处理的主要是相对路径，和单图片名的情况

# iCloud设置保持下载
默认vault保存在iCloud以实现同步，但是由于iCloud对mac空间优化，它会自动将不常访问的文件从本地移除，需要访问时再从云端下载，这导致了有些笔记打开缓慢。我们希望vault文件夹一直留在本地。

解决方案：
启动‘访达‘，并从侧边栏选择 iCloud 。找到您希望保留在 Mac 上的项目，然后右键/*按住 Control 键*单击该文件，接着**选择‘保持下载’**。


# 卡片学习法



## 笔记标签基本分类
1. 临时 fleeting：**自己的**一闪即逝的想法，创意，思考，需要进一步深挖和处理
	1. 这里我增加一个分类problem：还没有解决的问题和疑惑
	2. quote，单纯的原话复述，最好带上出处
	3. raw表示非常稚嫩原始的想法，没有仔细思考，想法也不明确
2. 文献 input：经过自己总结提炼的，来自于文本、视频等外部的信息和想法
3. 永久 permanent：经过多方面整合和继续推进的想法，比较成熟和完善
4. 项目 xx-project：特定project相关的内容

## 文件夹原本想法
1. 领域相关
2. 特定Project下的
问题是不同文件夹内的内容依然可能交叉，而笔记标签可以完全解决这个问题并且覆盖文件夹的功能。

# 参考资料
[每日笔记、日程管理、工作复盘——这是我钻研出的 Obsidian 八般武艺 - 少数派](https://sspai.com/post/72385)
[【Obsidian卡片盒笔记法】我如何利用Obsidian实现卡片盒笔记法|高级教程|Obsidian工作流分享\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1om4y1R7ad/?spm_id_from=333.337.search-card.all.click&vd_source=e92a724b260beb7547cd5d295ab87516)