---
title: markdown笔记/hexo记录
date: 2020-07-29 13:25:08
math: true
excerpt: 记录一些关于为hexo博客用typora写markdown的尝试
tags:
- instructions
- hexo
---

# 博客期待实现功能

**Hexo PDF问题**:能不能对于pdf进行显示 而且不是控件中移动 而是每页每页像图片一样放在博文里 不用滚动条

Hexo可以用api来实现返回不同的首页显示文字。可以增加显示每天天气、温度的功能？ => 只需要找到好用的天气API

# 初出茅庐

~~这typora怎么用微软输入法打第一个汉字的时候自动跑到Front-Matter字段去了 好烦~~

第一次使用markdown，记录一下 ~~复制来的~~

```undefined
*斜体文本*
_斜体文本_
**加粗文本**
***斜体加粗文本***
~~下划线文本~~
```

井号 `#` 加空格 产生标题

## Emoji

每个Emoji对应一个Unicode

在typora markdown里表现方法：

> &#x1f605；或者&#128517； (&#加编码加冒号为html里面标识特殊符号的方法；该表情Unicode为U+1F601，1F601前面加x表示十六进制/转为十进制)

&#x1f605;&#128517;

> : sweat_smile :   两冒号中间加英文 typora中的快速打法 其他地方可能不兼容

:sweat_smile:

> 直接mac输入法打出来的也是可以显示的

😅

## 数学公式

使用latex格式输入公式，*typora使用mathjax渲染*
$$
\{\frac{a}{(1+b)}\cdot2\pi\alpha\}
$$

\begin{gathered}实现公式全部居中对齐

\begin{align}配合&可以自定义对齐点，没有&时靠右对齐

**\*注意⚠️：换行时因为hexo默认渲染引擎对mathjax支持不好，编译时转义反斜杠，需要输入四个\而不是两个\才能正确在页面中实现换行。\***

[解决办法](https://wenghonghui.github.io/2020/03/05/hexo-mathjax/)：
hexo-renderer-marked换成hexo-renderer-kramed

> 在文章 Front-matter 里指定 `math: true` 才会在文章页启动公式转换，以便在页面不包含公式时提高加载速度

H<sub>2</sub>O——typora自带`<sub>`标签实现下标


## 代码块

这里截取一段微信小程序代码 来自`musicmood` ~~小程序真难做啊~~

```js
  getMood:function(){
    const db = wx.cloud.database()
    db.collection("musicmood").where({
      music:this.data.musicUrl,
      openid:this.data.openid
    }).get()
    .then(res=>{
      this.setData({
        timeSample:res.data[0].time,
        resonanceSample:res.data[0].resonance,
        feelingSample:res.data[0].feeling
      })
    })
    .catch(err=>{
      console.log(err)
    })
  }
```

-别来

-危险

-**前** **后** **夹** **击**

![累了.jpg](/img/hexo记录/QQ图片20200730152128.jpg)

## 图片问题

typora解析图片的根目录和hexo博客部署之后有点差别。typora可读入`![a](/a.jpg)`该相对路径下的图片。但是hexo部署之后只可读入`![a](a.jpg)`该相对路径下的图片。若需要在Front Matter部分加入`typora-root-url: ./$fileName`可以减少要删的，从删`$fileName/`到只删`/`，但实质没有区别。

如果有超多图片的可以考虑用正则表达式批量删除`/`

## 超链接

```
<https://www.baidu.com>
```

[百度][1]有链接标记，_即下面的数字1，也可以是任何文字_，便于作者自己统一更新文章链接

[1]:https://www.baidu.com	"有标识的超链接，百度网"

---

还有一种是脚注，与上面会标蓝的链接的注释不同。

百度[^1]

[^1]: 网址为www.baidu.com

------

不知道为什么分割线粗细随机

> 还有[百度]（https://www.baidu.com)

[百度](https://www.baidu.com/#dada )

---

这里也可以在括号中输入#(标题名称)做到**页面内跳转**

[实例](#超链接)

---

# 解决的Hexo问题

banner加载不了	=> 	***注意文件后缀名***

图片无法get	=>	本地预览的时候在文件夹里，实际生成网址在根目录，需要手动删去图片前文件夹名

能不能每次new的时候自动生成更多Front-Matter字段？ => 博客目录scaffolds文件夹下就是hexo new生成文件的模板

Hexo-generator-index的lib里的generator负责index页面的文章排序。

Hexo Front Matter字段都是YAML语言，冒号后面必须加空格，字段名本身不区分大小写；
如果想要多标签、多分类，两种写法:

1. 分行

```
tags:
- tag1
- tag2
```

2. 元组

```
tags: [tag1,tag2]
```

本地favicon预览不到 => safari不给localhost显示favicon，chrome可以

对网站大图标和小图标都进行了优化：Apple-Touch-Icon是显示在Safari书签栏的大LOGO。
图标搜索的优先级如下：

- 如果没有跟相应设备推荐尺寸一致的图标，那个会优先使用比推荐尺寸大，但最接近推荐尺寸的图标。
- 如果没有比推荐尺寸大的图标，会优先选择最接近推荐尺寸的图标。
- 如些有多个图标符合推荐尺寸，会优先选择包含关键字precomposed的图标。

比如知乎的Size有60x60,76x76,120x120,152x152
不周山博客的Size只有180x180但显示了