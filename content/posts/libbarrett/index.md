---
title: 'Ubuntu 编译安装 Libbarrett'
date: 2024-03-07T21:17:04+08:00
lastmod: 
draft: false
authors: [ethan]
description: 
tags: [libbarrett]
categories: [笔记]
series: 
series_weight: 
seriesNavigation: true
featuredImage: ""
featuredImagePreview: ""
hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: true
lightgallery: false # 如果设为 true, 文章中的图片将可以按照画廊形式呈现
---
<!--more-->

## 系统及软件版本

[libbarrett 3.0.x readme ](https://git.barrett.com/software/libbarrett)：

> This version of Libbarrett works with a non-real-time kernel (**a low-latency Ubuntu 20.04 kernel**) and should only be used when a hard real-time guarantee is not critical for your application.

## 编译安装 libbarrett

### 克隆仓库

```bash
cd && git clone https://git.barrett.com/software/libbarrett
```

### 安装依赖

```bash
cd ~/libbarrett/scripts && ~/libbarrett/scripts/install_dependencies.sh
```

这个过程中脚本会自动下载安装 lowlatency 内核等依赖，需要保证**良好**网络链接

脚本结束后重启进入系统，查看内核版本：

```bash
uname -r
```

输出内容应该以 `lowlatency` 结尾

### 安装 PCAN 驱动

```bash
sh ~/libbarrett/scripts/install_pcan.sh
```

PCAN-USB 不需要执行下面内容

> For PCAN-ISA only, manually configure the driver (not plug-and-play):
>
> ```bash
> sudo tee /etc/modprobe.d/pcan.conf <<EOF
> options pcan type=isa,isa io=0x300,0x320 irq=7,5
> install pcan modprobe --ignore-install pcan
> EOF
> echo 'pcan' |sudo tee -a /etc/modules-load.d/modules.conf
> ```

脚本执行完毕重启进入系统，检查 PCAN 驱动情况

执行 `cat /proc/pcan` 应当有类似以下有关 "can0" 的输出：

```
*------------- PEAK-System CAN interfaces (www.peak-system.com) -------------
*------------- Release_20210119_n (8.11.0) Mar  7 2024 20:57:07 --------------
*---------- [mod] [isa] [pci] [pec] [dng] [par] [usb] [pcc] [net] -----------
*--------------------- 1 interfaces @ major 238 found -----------------------
*n -type- -ndev- --base-- irq --btr- --read-- --write- --irqs-- -errors- status
32    usb   can0 ffffffff 000 0x001c 00000000 00000000 00000000 00000000 0x0000
```

### 编译安装 libbarrett 驱动

编译

```bash
export CC=/usr/bin/clang
export CXX=/usr/bin/clang++
cd ~/libbarrett && cmake .
make -j$(nproc)
```

安装

```bash
sudo make install
```

### （可选）编译例程

```bash
cd ~/libbarrett/examples && cmake .
make -j$(nproc)
```