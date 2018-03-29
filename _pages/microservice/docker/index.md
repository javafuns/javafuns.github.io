---
title: Hey, Docker!
date: '2017-06-01 18:43:00 +0800'
categories: docker
tags: docker
---

# Under the hood

Docker 基于 Linux Kernel 的 namespace 和 cgroup 实现:
* namespaces
    隔离资源，包括mount point，process id，network，user id，inter-process communication(进程间通信)，UTS(主机和域名) 等等。
* cgroup
    控制、限制资源的使用。

# Benefits

* 轻量 - lightweight
* 启动迅速 - fast-startup
* 保证开发测试部署环境一致
    make application deployable (as an image) so keep application running in a same environment in development, test, deployment stage
* portable - run anywhere
* 快速部署 - faster deployment
* 易于管理 - for easier management
