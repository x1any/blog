---
title: "Ubuntu 实时内核安装 NVIDIA 驱动"
subtitle: ""
date: 2025-03-25T13:37:01+08:00
lastmod: 2025-03-25T13:37:01+08:00
draft: false
authors: [x1any]
description: ""

tags: [ubuntu, nvidia, PREEMPT_RT]
categories: []
series: []
series_weight: 0

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: "https://img.x1an.org/2025/03/90bede40ae47b8a8ad7f636a402a76e2.png"
featuredImagePreview: "https://img.x1an.org/2025/03/90bede40ae47b8a8ad7f636a402a76e2.png"

toc:
  enable: true
math:
  enable: false
lightgallery: false
license: ""
---
<!--more-->

测试版本：ubuntu 22.04 + 5.15.0-1080-realtime + 535.230.02

步骤：
1. 下载 `NVIDIA .run` 驱动
2. 禁用 `nouveau` 驱动
3. 停用图形界面
4. 重启
5. 使用参数 `IGNORE_PREEMPT_RT_PRESENCE=1` 安装
6. 启用图形界面
7. 重启

```bash
curl -o nvidia-535.run -L https://cn.download.nvidia.com/XFree86/Linux-x86_64/535.230.02/NVIDIA-Linux-x86_64-535.230.02.run

echo "blacklist nouveau
options nouveau modeset=0" | sudo tee /etc/modprobe.d/blacklist-nouveau.conf

sudo update-initramfs -u
sudo systemctl set-default multi-user.target
sudo reboot

sudo IGNORE_PREEMPT_RT_PRESENCE=1 bash nvidia-535.run
sudo systemctl set-default graphical.target
sudo reboot
```
