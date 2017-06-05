---
layout: default
title: How to write a Dockerfile!
date: '2017-06-01 18:43:00 +0800'
categories: docker
tags: docker
---

# Overview
Dockerfile 是一个文本文件, 包含了用于装配 docker image 的一系列指令. 通过 docker build 从 Dockerfile 读取指令, 用户可以创建和生成一个 Docker image.
本文列举了可用于 Dockerfile 的命令.

# Dockerfile instructions

## FROM
Dockerfile 的第一条指令必须是 From, 用于指定从哪个 base image 开始创建.

NOTE: 某些情况下, 比如需要更改 escape 字符, 需使用 parser directive 指令并置于 Dockerfile 最顶部(FROM 指令之前)
例如:
```
# escape=` // default is backslash '\'

FROM <image>
```

## ENV
定义环境变量, 可以 $variable_name or ${variable_name} 形式在 Dockerfile 中引用.

例子:
```
ENV foo /bar
ENV <key>=<value> <key1>=<value1> ...
```
第一种形式只能定义一个 key/value pair, 第二种形式可以一次定义多个 key/value pair.

下列指令支持环境变量:
- ADD
- COPY
- ENV
- EXPOSE
- LABEL
- USER
- WORKDIR
- VOLUME
- STOPSIGNAL
及
- ONBUILD (when combined with one of the supported instructions above)

## RUN
RUN 命令有 2 种形式:
- RUN <command> (shell form, the command is run in a shell, which by default is /bin/sh -c on Linux or cmd /S /C on Windows)
- RUN ["executable", "param1", "param2"] (exec form)

exec 形式可用于 base image 不包含 shell 的情况. Shell 形式的默认 shell 可通过 SHELL 命令去更改.

使用 Shell 形式, 可以使用 \ 把单一命令写成多行形式.
## CMD

## .dockerignore file

# FAQ
### Dockerfile 中指令之间是否有依赖关系?
No, The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile. 

The core concepts of Docker where commits are cheap and containers can be created from any point in an image’s history, much like source control.

### RUN, CMD, ENTRYPOINT 区别

