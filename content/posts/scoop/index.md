---
title: 'Windows 系统下的包管理器 Scoop'
date: 2024-03-03T00:00:00+08:00
lastmod:
draft: false
authors: [ethan]
description: Scoop 是一款适用于 Windows 平台的命令行软件（包）管理工具
tags: [scoop]
categories: [工具]
series:
series_weight:
seriesNavigation: true
featuredImage: https://img.hsien.top/WNG1gT0qqxgntU92YNK2.png?imageMogr2/format/webp
featuredImagePreview: https://img.hsien.top/WNG1gT0qqxgntU92YNK2.png?imageMogr2/format/webp
hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: true
lightgallery: false # 如果设为 true, 文章中的图片将可以按照画廊形式呈现
---
<!--more-->

## 相关资源

* 使用方法：[scoop 包管理器的安装和相关技巧](https://www.cnblogs.com/linshengqian/p/15737833.html)
* 软件推荐：[Scoop上的软件推荐](https://blog.xqh.ma/_posts/2020-03-11-Scoop%E4%B8%8A%E7%9A%84%E8%BD%AF%E4%BB%B6%E6%8E%A8%E8%8D%90/)
* Bucket 目录：[Scoop Directory](https://rasa.github.io/scoop-directory/)

## 如何卸载

```powershell
scoop uninstall scoop
```

## 自定义安装

先允许 Powershell 执行本地脚本

```powershell
set-executionpolicy remotesigned -scope currentuser
```

安装 scoop

```powershell
$env:SCOOP='D:\Scoop'

# 先添加用户级别的环境变量 SCOOP
[environment]::setEnvironmentVariable('SCOOP',$env:SCOOP,'User')

# 下载安装 Scoop 
irm get.scoop.sh | iex
```

设置全局安装路径（官方文档不推荐）

```powershell
$env:SCOOP_GLOBAL='D:\GlobalScoopApps'

[environment]::setEnvironmentVariable('SCOOP_GLOBAL',$env:SCOOP_GLOBAL,'Machine')

# 取消
[Environment]::SetEnvironmentVariable('SCOOP_GLOBAL', $null, 'Machine')
```

## 建议安装的程序

```powershell
# 但 scoop 进行全局安装时需要使用到 sudo 命令
scoop install sudo

# scoop下载程序时支持使用 aria2 来加速下载
scoop install aria2

# 命令补全
scoop install scoop-completion
```

## 常用命令

```powershell
scoop help           # 查看帮助
scoop help <某个命令> # 具体查看某个命令的帮助

scoop install <app>   # 安装 APP
scoop uinstall <app>  # 卸载 APP

scoop list   # 列出已安装的 APP
scoop search # 搜索 APP
scoop status # 检查哪些软件有更新

scoop update               # 更新 Scoop 自身
scoop update <app1> <app2> # 更新某些app
scoop update *             # 更新所有 app （前提是需要在apps目录下操作）

scoop bucket known          # 通过此命令列出已知所有 bucket（软件源）
scoop bucket add bucketName # 添加某个 bucket

scoop cache rm <app> # 移除某个app的缓存
```

## 安装/卸载

```powershell
# 安装之前，通过 search 搜索 APP, 确定软件名称
scoop search  xxx

# 安装 APP
scoop install <app>

# 安装特定版本的 APP；语法 AppName@[version]，示例
scoop install git@2.23.0.windows.1

# 卸载 APP 
scoop uninstall <app> #卸载 APP
```

## 更新软件


```powershell
# 更新 Scoop 自身
scoop update

# 更新某些app
scoop update appName1 appName2

# 更新所有 app （可能需要在apps目录下操作）
scoop update *

# 禁止某程序更新
scoop hold <app>

# 允许某程序更新
scoop unhold <app>
```

## 清除缓存与旧版本

```powershell
# 查看所有以下载的缓存信息
scoop cache show

# 清除指定程序的下载缓存
scoop cache rm <app>

# 清除所有缓存
scoop cache rm *

# 删除某软件的旧版本
scoop cleanup <app>

# 删除全局安装的某软件的旧版本
scoop cleanup <app> -g

# 删除过期的下载缓存
scoop cleanup <app> -k
```

## 在同一程序的不同版本之间切换

```powershell
scoop reset [app]@[version]
```

## 其它命令

```powershell
# 显示某个app的信息
scoop info <app>

# 在浏览器中打开某app的主页
scoop home <app>

# 比如
scoop home git
```

## 添加软件源 Bucket

Scoop 默认的 Bucket 为 `main` ；官方维护的另一个 Bucket 为 `extras`，我们需要手动添加

```powershell
# bucket的用法
scoop bucket add|list|known|rm [<args>]

# 添加extras
scoop bucket add extras
```

* extras：[ScoopInstaller/Extras](https://github.com/ScoopInstaller/Extras)
* dorado：[chawyehsu/dorado](https://github.com/chawyehsu/dorado)
* ash258：[Ash258/Shovel-Ash258](https://github.com/Ash258/Shovel-Ash258)

```powershell
scoop bucket add extras
scoop bucket add dorado https://github.com/chawyehsu/dorado
scoop bucket add Ash258 https://github.com/Ash258/Shovel-Ash258
```

## 导出/导入 JSON

```powershell
scoop export > ~\scoop.json
scoop import ~\scoop.json
```