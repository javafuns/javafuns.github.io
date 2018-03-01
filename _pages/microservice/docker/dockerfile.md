---
layout: default
title: How to write a Dockerfile!
date: '2017-06-01 18:43:00 +0800'
categories: docker
tags: docker
---

* [Overview](#overview)
* [Dockerfile instructions](#dockerfile-instructions)
  * [FROM](#from)
  * [ENV](#env)
  * [RUN](#run)
  * [CMD](#cmd)
  * [ENTRYPOINT](#entrypoint)
    * [理解 CMD 和 ENTRYPOINT 如何交互](#%E7%90%86%E8%A7%A3-cmd-%E5%92%8C-entrypoint-%E5%A6%82%E4%BD%95%E4%BA%A4%E4%BA%92)
  * [LABEL](#label)
  * [MAINTAINER (deprecated)](#maintainer-deprecated)
  * [EXPOSE](#expose)
  * [ENV](#env)
  * [ADD](#add)
  * [COPY](#copy)
  * [VOLUME](#volume)
  * [USER](#user)
  * [WORKDIR](#workdir)
  * [ARG](#arg)
  * [ONBUILD](#onbuild)
  * [STOPSIGNAL](#stopsignal)
  * [HEALTHCHECK](#healthcheck)
  * [SHELL](#shell)
  * [.dockerignore file](#dockerignore-file)
* [FAQ](#faq)
  * [Dockerfile 中指令之间是否有依赖关系?](#dockerfile-%E4%B8%AD%E6%8C%87%E4%BB%A4%E4%B9%8B%E9%97%B4%E6%98%AF%E5%90%A6%E6%9C%89%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB)
  * [为什么要把多个命令 RUN apt-get update && apt-get install -y --force-yes apache2 写到一行?](#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%8A%8A%E5%A4%9A%E4%B8%AA%E5%91%BD%E4%BB%A4-run-apt-get-update-apt-get-install--y---force-yes-apache2-%E5%86%99%E5%88%B0%E4%B8%80%E8%A1%8C)
  * [RUN, CMD, ENTRYPOINT 区别](#run-cmd-entrypoint-%E5%8C%BA%E5%88%AB)
  * [ADD 和 COPY 区别](#add-%E5%92%8C-copy-%E5%8C%BA%E5%88%AB)

# Overview

Dockerfile 是一个文本文件, 包含了用于装配 docker image 的一系列指令. 通过 docker build 从 Dockerfile 读取指令, 用户可以创建和生成一个 Docker image.
本文列举了可用于 Dockerfile 的命令.

# Dockerfile instructions

## FROM

Dockerfile 的第一条指令必须是 From, 用于指定从哪个 base image 开始创建.

NOTE: 某些情况下, 比如需要更改 escape 字符, 需使用 parser directive 指令并置于 Dockerfile 最顶部(FROM 指令之前)

例如:
```bash
# escape=` // default is backslash '\'

FROM <image>
```

除了选择现有镜像为基础镜像外, Docker 还存在一个特殊的镜像, 名为 scratch. 这个镜像是虚拟的概念, 并不实际存在, 它表示一个空白的镜像.
```bash
FROM scratch
```
如果你以 scratch 为基础镜像的话, 意味着你不以任何镜像为基础, 接下来所写的指令将作为镜像第一层开始存在.

## ENV

定义环境变量, 可以 $variable_name or ${variable_name} 形式在 Dockerfile 中引用.

例子:
```bash
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

使用 Shell 形式的时候, 可以使用 \ 把一行命令写成多行便于阅读.

Note:  
与 shell 形式不同, exec 形式不调用 shell 命令. 这意味着 shell 形式不会有常规的 shell 处理. 例如, RUN [ "echo", "$HOME" ] 不会为 $HOME 变量做替换. 如果想要有 shell 处理, 那么或者使用 shell 形式或者直接执行一个 shell, 例如: RUN [ "sh", "-c", "echo $HOME" ]. 

## CMD

CMD 命令的主要用意是为 container 提供默认的命令或参数, 当运行一个 image 的时候, 该命令就会被执行.

CMD 命令有 3 种形式:
- CMD ["executable","param1","param2"] (exec form, this is the preferred form)
- CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
- CMD command param1 param2 (shell form)

Note:
- 如果 CMD 是用于为 ENTRYPOINT 指令提供默认参数, CMD 和 ENTRYPOINT 指令都应该以 JSON array 格式在 Dockerfile 中定义.
- 如果用户在 docker run 命令中指定了参数, 那么这些参数将覆盖用 CMD 指令所定义的默认值.
- 在 Dockerfile 里只能有一个 CMD 指令. 如果列出了多个 CMD 指令, 那么也只有最后一个 CMD 会起作用.
- 不要混淆 RUN 和 CMD. RUN 实际上只在 build time 运行命令并提交结果; CMD 在 build time 并不执行任何命令, 但为 image 指定了运行 container 时想要执行的命令.

## ENTRYPOINT

ENTRYPOINT 指令有 2 种形式:
```
ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)
ENTRYPOINT command param1 param2 (shell form)
```

ENTRYPOINT 指令允许你配置 container 作为一个可执行文件去运行.

例如, 以下命令会启动 nginx 并监听 80 端口:
```bash
docker run -i -t --rm -p 80:80 nginx
```

docker run <image> 的命令行参数会附加在 exec 形式的 ENTRYPOINT 命令之后, 且会覆盖掉所有通过 CMD 指令指定的参数. 这种方式允许允许传递参数给 entry point, 即 docker run <image> -d 会传递 -d 参数给 entry point. 你可以通过 docker run --entrypoint flag 覆盖 ENTRYPOINT 指令.

Shell 形式可防止使用任何 CMD 或 run 命令行参数, 但有一个缺点就是你的 ENTRYPOINT 是以 /bin/sh -c 的子命令开始的, 这种情况下不会传递信号(pass signals). 这意味着执行进程不是 container’s PID 1 - 并且不会接收 Unix signals - 所以你的执行进程不会从 docker stop <contaienr> 命令接收到 SIGTERM.

Dockerfile 文件中只有最后一个 ENTRYPOINT 指令起作用, 之前的 ENTRYPOINT 指令会被忽略.

### 理解 CMD 和 ENTRYPOINT 如何交互

CMD 和 ENTRYPOINT 指令都定义了在运行 container 时执行何种命令. 如下规则描述了它们之间如何协作.
- Dockerfile should specify at least one of CMD or ENTRYPOINT commands.
- ENTRYPOINT should be defined when using the container as an executable.
- CMD should be used as a way of defining default arguments for an ENTRYPOINT command or for executing an ad-hoc command in a container.
- CMD will be overridden when running the container with alternative arguments.

下表说明了不同的 ENTRYPOINT/CMD 命令组合是如何被执行的:

|                            | No ENTRYPOINT              | ENTRYPOINT exec_entry p1_entry | ENTRYPOINT [“exec_entry”, “p1_entry”]          |
|:---------------------------|:---------------------------|:-------------------------------|:-----------------------------------------------|
| No CMD                     | error, not allowed         | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry                            |
| CMD [“exec_cmd”, “p1_cmd”] | exec_cmd p1_cmd            | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry exec_cmd p1_cmd            |
| CMD [“p1_cmd”, “p2_cmd”]   | p1_cmd p2_cmd              | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry p1_cmd p2_cmd              |
| CMD exec_cmd p1_cmd        | /bin/sh -c exec_cmd p1_cmd | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry /bin/sh -c exec_cmd p1_cmd |

## LABEL

LABEL 指令可为 image 添加元数据. 一个 label 是一个 key/value pair. 一个 image 可以有多个 LABEL. Docker 官方建议多个 labels 使用一个 LABEL 指令去定义.

如果使用多个 LABEL 指令, 每个 LABEL 指令都会产生一个新的 layer, 这会导致多个效率低下的 image.

```bash
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```

## MAINTAINER (deprecated)

Docker 官方建议使用 LABEL 指令:
```bash
LABEL maintainer "javafuns@about.me"
```

## EXPOSE

```bash
EXPOSE <port> [<port>...]
```

EXPOSE 指令告诉 Docker 该 container 在运行时所监听的网络端口. EXPOSE 指令并不会使该端口对宿主机(host)可见(accessible). 要想让宿主机访问 container 的端口, 在 docker run 时必须使用 -p flag 公开一组端口范围或使用 -P flag 公开所有 exposed ports. expose 端口和公开给外部访问的端口不要求是同一个 number.

## ENV

```bash
ENV <key> <value>
ENV <key>=<value> ...
```

ENV 指令设置环境变量. 设置过的环境变量对后续的 Dockerfile 命令都是可见的.

与 LABEL 指令一样, 如果想设置多个环境变量, 尽可能使用一个 ENV 指令完成设置.

## ADD

ADD 指令复制文件, 目录或者远程文件 (remote file URLs) 并把它们添加到 image 文件系统中.

ADD 指令有 2 种形式:
```bash
ADD <src>... <dest>
ADD ["<src>",... "<dest>"] (this form is required for paths containing whitespace)
```

可以指定多个 <src> resource, 但是如果它们是文件或者目录的话, 它们必须是相对于当前编译目录的相对目录 (relative to the source directory that is being built (the context of the build)).

\<dest\> 是 container 中的绝对路径, 或者是相对 WORKDIR 的相对路径.

ADD 指令须遵从如下规则:
- The <src> path must be inside the context of the build; you cannot ADD ../something /something, because the first step of a docker build is to send the context directory (and subdirectories) to the docker daemon.
- If <src> is a URL and <dest> does not end with a trailing slash, then a file is downloaded from the URL and copied to <dest>.
- If <src> is a URL and <dest> does end with a trailing slash, then the filename is inferred from the URL and the file is downloaded to <dest>/<filename>. For instance, ADD http://example.com/foobar / would create the file /foobar. The URL must have a nontrivial path so that an appropriate filename can be discovered in this case (http://example.com will not work).
- If <src> is a directory, the entire contents of the directory are copied, including filesystem metadata.
Note: The directory itself is not copied, just its contents.
- If <src> is a local tar archive in a recognized compression format (identity, gzip, bzip2 or xz) then it is unpacked as a directory. Resources from remote URLs are not decompressed. When a directory is copied or unpacked, it has the same behavior as tar -x, the result is the union of:
  - Whatever existed at the destination path and
  - The contents of the source tree, with conflicts resolved in favor of “2.” on a file-by-file basis.  
Note: Whether a file is identified as a recognized compression format or not is done solely based on the contents of the file, not the name of the file. For example, if an empty file happens to end with .tar.gz this will not be recognized as a compressed file and will not generate any kind of decompression error message, rather the file will simply be copied to the destination.
- If <src> is any other kind of file, it is copied individually along with its metadata. In this case, if <dest> ends with a trailing slash /, it will be considered a directory and the contents of <src> will be written at <dest>/base(<src>).
- If multiple <src> resources are specified, either directly or due to the use of a wildcard, then <dest> must be a directory, and it must end with a slash /.
- If <dest> does not end with a trailing slash, it will be considered a regular file and the contents of <src> will be written at <dest>.
- If <dest> doesn’t exist, it is created along with all missing directories in its path.

## COPY

COPY 指令有 2 种形式:
```bash
COPY <src>... <dest>
COPY ["<src>",... "<dest>"] (this form is required for paths containing whitespace)
```

COPY 指令复制新文件和目录并把它们加入到 container 文件系统中.

可以指定多个 <src> resource, 但是它们必须是相对于当前编译目录的相对目录.

\<dest\> 是 container 中的绝对路径, 或者是相对 WORKDIR 的相对路径.
```bash
COPY test relativeDir/   # adds "test" to `WORKDIR`/relativeDir/
COPY test /absoluteDir/  # adds "test" to /absoluteDir/
```

COPY 遵从如下规则:
- The <src> path must be inside the context of the build; you cannot COPY ../something /something, because the first step of a docker build is to send the context directory (and subdirectories) to the docker daemon.
- If <src> is a directory, the entire contents of the directory are copied, including filesystem metadata.
Note: The directory itself is not copied, just its contents.
- If <src> is any other kind of file, it is copied individually along with its metadata. In this case, if <dest> ends with a trailing slash /, it will be considered a directory and the contents of <src> will be written at <dest>/base(<src>).
- If multiple <src> resources are specified, either directly or due to the use of a wildcard, then <dest> must be a directory, and it must end with a slash /.
- If <dest> does not end with a trailing slash, it will be considered a regular file and the contents of <src> will be written at <dest>.
- If <dest> doesn’t exist, it is created along with all missing directories in its path.

## VOLUME

```bash
VOLUME ["/var/log/"]
VOLUME /var/log
VOLUME /var/log /var/db
```

VOLUME 指令使用给定名字创建挂载点(mount point) 并且这个挂载点持有来自外部宿主机或其它 container 的卷(volume). 

docker run 命令在创建 container 时, 挂载点所使用到的目录或 volume 里的文件(如有)对当前挂载点仍然可见.

NOTE:
- 想加载特定的宿主机目录, 则必须在创建或运行 container 时才能指定. Dockerfile 里 VOLUME 命令所创建加载点使用的宿主机目录是形如 "/var/lib/docker/vfs/dir/cde167197ccc3e138a14f1a4f".
- 如果在 volume 已经声明之后有任何编译步骤改变了 volume 里的数据, 这些改变会被丢弃.

More: [深入理解 Docker Volume](http://dockone.io/article/128)

## USER

```bash
USER daemon
```

USER 指令设置 user name 或 UID, 以该身份运行 image 及 Dockerfile 中在 USER 指令之后的任何 RUN, CMD 和 ENTRYPOINT 指令.


## WORKDIR

```bash
WORKDIR /path/to/WORKDIR
```

WORKDIR 指令为它之后声明的 RUN, CMD, ENTRYPOINT, COPY 和 ADD 指令设置工作目录. 如果 WORKDIR 目录不存在, 它仍会被创建, 甚至如果随后的 Dockerfile 指令根本没有用到它.

在 Dockerfile 中这个指令可以使用多次. 如果提供的是相对路径, 那么它是相对于之前 WORKDIR 指令的目录.

WORKDIR 指令能解析在它之前所设置的环境变量.

## ARG

```bash
ARG <name>[=<default value>]
```

## ONBUILD

```bash
ONBUILD [INSTRUCTION]
```

ONBUILD 指令向 image 中添加了一个 trigger 指令, 这个 trigger 指令会在这个 image 作为 base 去创建新的 image 时被执行.
任何编译指令都可以注册为 trigger.
例如有如下 Dockerfile:
```bash
[...]
ONBUILD ADD . /app/src
ONBUILD RUN /usr/local/bin/python-build --dir /app/src
[...]
```

当基于这个 Dockerfile 的 image 去创建新的 image 时, 作为处理 FROM 指令的一部分, 会查找 ONBUILD 的 trigger 并按顺序执行. 如果 trigger 执行失败, FROM 指令也会被终止; 如果 trigger 执行成功, FROM 指令也执行成功, 则整个创建过程像往常一样继续下去.

## STOPSIGNAL

```bash
STOPSIGNAL signal
```

STOPSIGNAL 指令设置系统调用信号发送给 container 通知其退出. 信号可以是数字如 9, 也可以是信号名称如 SIGKILL.

## HEALTHCHECK

HEALTHCHECK 指令有 2 种形式:
```bash
HEALTHCHECK [OPTIONS] CMD command (通过在 container 中运行命令去检查 container health)
HEALTHCHECK NONE (禁止从 base image 继承的任何 healthcheck)
```

HEALTHCHECK 指令告诉 Docker 怎样去测试容器是否还正常工作.

当一个 container 指定了 healthcheck, 它的正常状态值后会带有健康状态. 这个值初始是 starting. 当 health check 成功, 它会变成 healthy (不论之前是什么状态). 当持续多次(特定次数)失败后, 它会变为 unhealthy.

在 CMD 之前的 options 可以是:
```bash
--interval=DURATION (default: 30s)
--timeout=DURATION (default: 30s)
--retries=N (default: 3)
```

只有在连续多次 health check 失败后, 才会认为 container 是 unhealthy.

在一个 Dockerfile 里只能有一个 HEALTHCHECK 指令. 如果列出了多个, 那么也只有最后那个 HEALTHCHECK 指令才会真正有效.

CMD 关键字后的命令可以是一个 shell 命令 (e.g. HEALTHCHECK CMD /bin/check-running) 也可以是一个 exec 数组 (跟 ENTRYPOINT 类似的).

命令的退出状态表明 container 的健康状况. 可能的值有:
- 0: success - the container is healthy and ready for use
- 1: unhealthy - the container is not working correctly
- 2: reserved - do not use this exit code

例如, 每 5 分钟检查一次 web server 是否还能在 3s 内生成网站主页:
```bash
HEALTHCHECK --interval=5m --timeout=3s CMD curl -f http://localhost/ || exit 1
```

## SHELL

## .dockerignore file

# FAQ

## Dockerfile 中指令之间是否有依赖关系?

No, The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile. 

The core concepts of Docker where commits are cheap and containers can be created from any point in an image’s history, much like source control.

## 为什么要把多个命令 RUN apt-get update && apt-get install -y --force-yes apache2 写到一行?

cache 失效问题:
- https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#build-cache
- https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#run

## RUN, CMD, ENTRYPOINT 区别

## ADD 和 COPY 区别
