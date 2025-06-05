---
title: 'Ubuntu 使用 CMake 编译安装 Libhv'
date: 2024-03-07T16:46:11+08:00
lastmod: 
draft: false
authors: [x1any]
description: 
tags: [libhv]
categories: [笔记]
series: 
series_weight: 
seriesNavigation: true
featuredImage: 
featuredImagePreview: 
hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: true
lightgallery: false # 如果设为 true, 文章中的图片将可以按照画廊形式呈现
---
<!--more-->

## 编译安装

```sh
git clone --depth=1 -b v1.3.2 https://github.com/ithewei/libhv.git
cd libhv
cmake -S . -B build
sudo cmake --build build --target install
sudo ldconfig
```

## 说明

下载 `v1.3.2`​ 版本的 libhv

```bash
git clone --depth=1 -b v1.3.2 https://github.com/ithewei/libhv.git
```

切换工作目录

```bash
cd libhv
```

配置 cmake

```bash
cmake -S . -B build
```

编译，安装

```bash
sudo cmake --build build --target install
```

输出：

```
Install the project...
-- Install configuration: ""
-- Installing: /usr/local/lib/libhv.so
-- Set runtime path of "/usr/local/lib/libhv.so" to ""
-- Installing: /usr/local/lib/libhv_static.a
-- Installing: /usr/local/include/hv/hv.h
-- Installing: /usr/local/include/hv/hconfig.h
-- Installing: /usr/local/include/hv/hexport.h
-- Installing: /usr/local/include/hv/hplatform.h
-- Installing: /usr/local/include/hv/hdef.h
-- Installing: /usr/local/include/hv/hatomic.h
-- Installing: /usr/local/include/hv/herr.h
-- Installing: /usr/local/include/hv/htime.h
-- Installing: /usr/local/include/hv/hmath.h
-- Installing: /usr/local/include/hv/hbase.h
-- Installing: /usr/local/include/hv/hversion.h
-- Installing: /usr/local/include/hv/hsysinfo.h
-- Installing: /usr/local/include/hv/hproc.h
-- Installing: /usr/local/include/hv/hthread.h
-- Installing: /usr/local/include/hv/hmutex.h
-- Installing: /usr/local/include/hv/hsocket.h
-- Installing: /usr/local/include/hv/hlog.h
-- Installing: /usr/local/include/hv/hbuf.h
-- Installing: /usr/local/include/hv/hmain.h
-- Installing: /usr/local/include/hv/hendian.h
-- Installing: /usr/local/include/hv/hssl.h
-- Installing: /usr/local/include/hv/hloop.h
-- Installing: /usr/local/include/hv/nlog.h
-- Installing: /usr/local/include/hv/base64.h
-- Installing: /usr/local/include/hv/md5.h
-- Installing: /usr/local/include/hv/sha1.h
-- Installing: /usr/local/include/hv/hmap.h
-- Installing: /usr/local/include/hv/hstring.h
-- Installing: /usr/local/include/hv/hfile.h
-- Installing: /usr/local/include/hv/hpath.h
-- Installing: /usr/local/include/hv/hdir.h
-- Installing: /usr/local/include/hv/hurl.h
-- Installing: /usr/local/include/hv/hscope.h
-- Installing: /usr/local/include/hv/hthreadpool.h
-- Installing: /usr/local/include/hv/hasync.h
-- Installing: /usr/local/include/hv/hobjectpool.h
-- Installing: /usr/local/include/hv/ifconfig.h
-- Installing: /usr/local/include/hv/iniparser.h
-- Installing: /usr/local/include/hv/json.hpp
-- Installing: /usr/local/include/hv/singleton.h
-- Installing: /usr/local/include/hv/ThreadLocalStorage.h
-- Installing: /usr/local/include/hv/Buffer.h
-- Installing: /usr/local/include/hv/Channel.h
-- Installing: /usr/local/include/hv/Event.h
-- Installing: /usr/local/include/hv/EventLoop.h
-- Installing: /usr/local/include/hv/EventLoopThread.h
-- Installing: /usr/local/include/hv/EventLoopThreadPool.h
-- Installing: /usr/local/include/hv/Status.h
-- Installing: /usr/local/include/hv/TcpClient.h
-- Installing: /usr/local/include/hv/TcpServer.h
-- Installing: /usr/local/include/hv/UdpClient.h
-- Installing: /usr/local/include/hv/UdpServer.h
-- Installing: /usr/local/include/hv/httpdef.h
-- Installing: /usr/local/include/hv/wsdef.h
-- Installing: /usr/local/include/hv/http_content.h
-- Installing: /usr/local/include/hv/HttpMessage.h
-- Installing: /usr/local/include/hv/HttpParser.h
-- Installing: /usr/local/include/hv/WebSocketParser.h
-- Installing: /usr/local/include/hv/WebSocketChannel.h
-- Installing: /usr/local/include/hv/HttpServer.h
-- Installing: /usr/local/include/hv/HttpService.h
-- Installing: /usr/local/include/hv/HttpContext.h
-- Installing: /usr/local/include/hv/HttpResponseWriter.h
-- Installing: /usr/local/include/hv/WebSocketServer.h
-- Installing: /usr/local/include/hv/HttpClient.h
-- Installing: /usr/local/include/hv/requests.h
-- Installing: /usr/local/include/hv/axios.h
-- Installing: /usr/local/include/hv/AsyncHttpClient.h
-- Installing: /usr/local/include/hv/WebSocketClient.h
-- Installing: /usr/local/lib/cmake/libhv/libhvConfig.cmake
-- Installing: /usr/local/lib/cmake/libhv/libhvConfig-noconfig.cmake
```

可以看出安装路径为

* include: `/usr/local/include`
* 静态库 & 动态库: `/usr/local/lib`

更新系统配置

```bash
sudo ldconfig
```
