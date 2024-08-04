---
title: "快速搭建一个 Hugo 站点 (DoIt 主题)"
subtitle: ""
date: 2024-08-04T21:44:50+08:00
lastmod: 2024-08-04T21:44:50+08:00
draft: false
authors: [ethan]
description: ""

tags: [hugo]
categories: [笔记]
series: []
series_weight: 0

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: ""
featuredImagePreview: ""

toc:
  enable: true
math:
  enable: false
lightgallery: false
license: ""
---
<!--more-->

## 创建新站点

```bash
hugo new site blog
cd blog
git init
git submodule add https://github.com/HEIGE-PCloud/DoIt.git themes/DoIt
git submodule update --remote --merge
```

## 基础配置

详细配置请参考: [主题文档](https://hugodoit.pages.dev/zh-cn/theme-documentation-basics/)

```toml
# ./hugo.toml
baseURL = "http://example.org/"
# [en, zh-cn, fr, ...] 设置默认的语言
defaultContentLanguage = "zh-cn"
# 网站语言, 仅在这里 CN 大写
languageCode = "zh-CN"
# 是否包括中日韩文字
hasCJKLanguage = true
# 网站标题
title = "我的全新 Hugo 网站"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "DoIt"

[params]
# DoIt 主题版本
version = "0.2.X"

[menu]
[[menu.main]]
identifier = "posts"
# 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
pre = ""
# 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
post = ""
name = "文章"
url = "/posts/"
# 当你将鼠标悬停在此菜单链接上时, 将显示的标题
title = ""
weight = 1
[[menu.main]]
identifier = "tags"
pre = ""
post = ""
name = "标签"
url = "/tags/"
title = ""
weight = 2
[[menu.main]]
identifier = "categories"
pre = ""
post = ""
name = "分类"
url = "/categories/"
title = ""
weight = 3

# Hugo 解析文档的配置
[markup]
# 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
[markup.highlight]
# false 是必要的设置 (https://github.com/dillonzq/LoveIt/issues/158)
noClasses = false

```

## 新建帖子

```bash
hugo new posts/hello-world/index.md
hugo serve --disableFastRender -D
```

## 部署

[Hugo · Cloudflare Pages docs](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/)

环境变量：

```
HUGO_VERSION = 0.122.0
```