---
title: 自定义Fluid主题
date: '2021-06-06 18:51:42'
math: false
excerpt: 对于本博客主题Fluid的客制化更改。
tags:
- instructions
- hexo
---

现在的根目录下`_config.fluid.yml`才是主题配置文件，覆盖themes/fluid中的配置文件。记得主题大版本更新时需要注意config内容是否随之更改。

# 对Fluid主题的修改

**强烈推荐博客设置`updated_option: 'date'`**。否则下列修改启用后如果进行版本管理（如git pull)会导致涉及的所有未设置updated字段的博文全都自动显示updated时间为文件修改时间（如git pull时间），而非正常情况：若无updated字段/updated时间不晚于date（按天比较），则不显示updated时间。

改动目标：

1. 首页的每篇文章除了创建时间之外还能显示文档更新时间。
2. 点进每篇文章之后都会显示创建时间和更新时间。

## 1

主题配置config文件中post_meta加入`updated: true`

在主题的layout文件夹index.ejs内的`class="index-btm post-metas"`的代码块加入下列代码，让index页面显示文章更新时间。

```
<% if(theme.index.post_meta.updated && (post.updated > post.date)) { %>
	<div class="post-meta mr-3">
    <i class="iconfont icon-pen"></i>
    <time datetime="<%= full_date(post.updated, 'YYYY-MM-DD HH:mm') %>">
      <%- date(post.updated, config.date_format) %>
    </time>
	</div>
<% } %>
```

## 2

主题配置config文件中post:meta:date部分加入updated选项。

```
# 文章日期，优先根据 front-matter 里 date 字段，其次是 md 文件日期
# updated_enable 显示 front-matter 里 updated 字段，如果updated晚于date。如果enable，则用两个单词区别两行日期。
date:
      enable: true
      updated_enable: true
      date_updated_words: ['创建', '更新']
      # 格式参照 ISO-8601 日期格式化
      # ISO-8601 date format
      # See: http://momentjs.cn/docs/#/parsing/string-format/
      format: "LL a"
```

找到_partial文件夹内post-meta.ejs，增加文章最近修改时间。显示updated时间时同时也要显示两个提示词来辨别两行日期，因为图标信息还是不够明确。

```
<div class="mt-1">
      <span class="post-meta">
        <i class="iconfont icon-date-fill" aria-hidden="true"></i>
        <time datetime="<%= full_date(page.date, 'YYYY-MM-DD HH:mm') %>" pubdate>
          <%= full_date(page.date, theme.post.meta.date.format) %>
        </time>
        <% if (theme.post.meta.date.updated_enable && (page.updated > page.date)) { %>
          <%- theme.post.meta.date.date_updated_words[0] %>
        <% } %>
      </span>
    </div>
    
    <div class="mt-1">
      <% if (theme.post.meta.date.updated_enable && (page.updated > page.date)) { %>
        <span class="post-meta">
          <i class="iconfont icon-pen" aria-hidden="true"></i>
          <time datetime="<%= full_date(page.updated, 'YYYY-MM-DD HH:mm') %>" pubdate>
            <%= full_date(page.updated, theme.post.meta.date.format) %>
          </time>
          <%- theme.post.meta.date.date_updated_words[1] %>
        </span>
      <% } %>
    </div>
```

## 效果

![](/img/FluidModification/截屏2022-01-04%2000.50.01.png)
![](/img/FluidModification/截屏2022-01-04%2000.50.27.png)