---
layout: post
title: '在WSL中使用docker'
date: 2020-05-21 12:47:24 +0800
tags:
  - Docker
  - WSL
---
>环境
>* 系统: Windows-10.0.18362.815
>* Docker: 19.03.8
>* Docker for Windows: 2.2.0.4（43472）


由于Docker在WSL下运行daemon守护进程有问题，所以仅仅在WSL中安装完docker后是没法用的，解决办法如下：
1. 在Docker Desktop中启用 2375 端口，暴露daemon进程；
2. 修改WSL中的docker环境变量 `DOCKER_HOST=tcp://localhost:2375`
3. 重启

