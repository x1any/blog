---
title: "OpenWrt 使用最新版 tailscale (ext4 持久化)"
subtitle: ""
date: 2024-08-04T21:15:47+08:00
lastmod: 2024-08-04T21:15:47+08:00
draft: false
authors: [ethan]
description: ""

tags: [openwrt, tailscale]
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

## 脚本 `/etc/init.d/tailscale`  

用于管理 tailscaled 服务，[来源](https://github.com/adyanth/openwrt-tailscale-enabler/blob/main/etc/init.d/tailscale)，内容如下：

```bash
#!/bin/sh /etc/rc.common

# Copyright 2020 Google LLC.
# SPDX-License-Identifier: Apache-2.0

USE_PROCD=1
START=99
STOP=1

start_service() {
  procd_open_instance
  procd_set_param command /usr/bin/tailscaled

  # Set the port to listen on for incoming VPN packets.
  # Remote nodes will automatically be informed about the new port number,
  # but you might want to configure this in order to set external firewall
  # settings.
  procd_append_param command --port 41641

  # OpenWRT /var is a symlink to /tmp, so write persistent state elsewhere.
  procd_append_param command --state /etc/config/tailscaled.state
  
  # Persist files for TLS cert & Taildrop files
  procd_append_param command --statedir /etc/tailscale/

  procd_set_param respawn
  procd_set_param stdout 1
  procd_set_param stderr 1

  procd_close_instance
}

stop_service() {
  /usr/bin/tailscaled --cleanup
}
```

## 持久化

原仓库是通过每次执行 `tailscale up` 的时候，调用下载脚本将二进制文件下载到 `/tmp` 中，由于我的 `OpenWrt` 是跑在 PVE 中的，使用了 `ext4` 文件系统，可以直接将二进制文件安装到系统中

下载对应版本的二进制文件，复制到 `/usr/bin` 路径下：


```bash
cd /tmp
curl -LO https://pkgs.tailscale.com/stable/tailscale_1.70.0_amd64.tgz
tar -xzvf tailscale_1.70.0_amd64.tgz
```

需要安装依赖

```bash
opkg update
opkg install iptables-nft # ≥22.03
```

启动服务

```bash
/etc/init.d/tailscale start
/etc/init.d/tailscale enable
tailscale up --netfilter-mode=off --advertise-routes=10.0.0.0/24 --accept-routes
```