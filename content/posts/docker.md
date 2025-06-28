---
title: Docker
subtitle:
date: 2025-05-08T15:30:44+08:00
slug: c41c58c
draft: true
author:
  name:
  link:
  email:
  avatar:
description:
keywords:
license:
comment: false
weight: 0
tags:
  - draft
categories:
  - draft
hiddenFromHomePage: false
hiddenFromSearch: false
hiddenFromRelated: false
hiddenFromFeed: false
summary:
resources:
  - name: featured-image
    src: featured-image.jpg
  - name: featured-image-preview
    src: featured-image-preview.jpg
toc: true
math: false
lightgallery: false
password:
message:
repost:
  enable: true
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->


## docker的安装
docker分为桌面版（desktop）和非桌面版（docker引擎engine），在官网上有详细的桌面版安装，但是非桌面版安装却有点含糊不清，maybe是我的问题。

1. 安装前先卸载之前的docker（如果有的话）
```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```
2. 安装必要的支持
```shell
sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release
```
3. 添加docker官方GPG key（或者使用别的gpg key）

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
4. 添加apt源

```shell
#docker官方源
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


#阿里apt源
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

5. 更新源
```shell
apt update

apt-get update
```

6. 安装docker
```shell
# 安装
sudo apt install docker-ce docker-ce-cli containerd.io

#查看版本
sudo docker version

#查看运行状态
sudo systemctl status docker
```

## 测试docker以及设置镜像
okay，在安装完docker之后让我们测试一下docker能否成功使用

```shell
docker run hello-world
```

不出意外，它报错了

```
root@VM-0-11-ubuntu:/home/july# docker run hello-worldUnable to find image 'hello-world:latest' locallydocker: Error response from daemon: Get "https://registry-1.docker.io/v2/": context deadline exceeded (Client.Timeout exceeded while awaiting headers)Run 'docker run --help' for more information
```
这倒是不出意外，它提示我们无法在官网上拉取镜像，至于更细致的原因这里不再探究，这里拉取不到我们换源就行了。

### 换源

```shell
# 首先创建一个配置文件

touch /etc/docker/daemon.json

vim /etc/docker/daemon.json
```
加入以下内容
``` json
{
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "https://hub-mirror.c.163.com",
    "https://mirror.baidubce.com",
    "https://docker.m.daocloud.io/",
    "https://huecker.io/",
    "https://dockerhub.timeweb.cloud",
    "https://noohub.ru/",
    "https://dockerproxy.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://docker.nju.edu.cn",
    "https://xx4bwyg2.mirror.aliyuncs.com",
    "http://f1361db2.m.daocloud.io",
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn"
  ]
}

```
这里的json文件格式不要错误，否则docker会重启失败，可以通过下载`jq`来进行测试。

```shell
apt-get install -y jq

jq . /etc/docker/daemon.json
```

之后重启docker服务并再次运行。

```shell
systemctl restart docker

docker run hello-world

# 输出
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
e6590344b1a5: Pull complete 
Digest: sha256:c41088499908a59aae84b0a49c70e86f4731e588a737f1637e73c8c09d995654
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```
哦对了，最好也修改一下dns服务，防止解析不到这些源。

```
vim /etc/resolv.conf

# 添加
nameserver 114.114.114.114
```

## docker使用
### hello world

Docker 允许你在容器内运行应用程序， 使用 docker run 命令来在容器内运行一个应用程序。
```shell
docker run ubuntu:15.10 /bin/echo "hello world"

# 完整结果
Unable to find image 'ubuntu:15.10' locally
15.10: Pulling from library/ubuntu
7dcf5a444392: Pull complete 
759aa75f3cee: Pull complete 
3fa871dc8a2b: Pull complete 
224c42ae46e7: Pull complete 
Digest: sha256:02521a2d079595241c6793b2044f02eecf294034f31d6e235ac4b2b54ffc41f3
Status: Downloaded newer image for ubuntu:15.10
hello world
```

各个参数的意义：

1. docker Docker的二进制执行文件
2. run 与前面的docker组合来运行一个容器
3. ubuntu:15.10 要运行的镜像，docker首先在本地主机上查找镜像是否存在，如果不存在，Docker 就会从镜像仓库 Docker Hub 下载公共镜像。
4. /bin/ehco "hello world" 在启动的容器里执行命令

### 交互式容器
上面的hello world没有交互的功能，我们可以通过一些参数来得到一个终端
```shell
docker run -i -t ubuntu:15.10 /bin/bash

# 结果如下
root@VM-0-11-ubuntu:/# docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED      STATUS      PORTS     NAMES
473b9a4bf792   ubuntu:15.10   "/bin/bash"   2 days ago   Up 2 days             compassionate_chaum
root@VM-0-11-ubuntu:/# docker run -i -t ubuntu:15.10 /bin/bash
root@d3f7ae6c4080:/# whoami
root
root@d3f7ae6c4080:/# ps
    PID TTY          TIME CMD
      1 pts/0    00:00:00 bash
     11 pts/0    00:00:00 ps
root@d3f7ae6c4080:/# docker 
bash: docker: command not found

# 注意此时用户的主机名称已经改变
#使用exit或者ctrl+d退出终端
```

参数解析：
1. -i 允许你对容器内的标准输入（STDIN）进行交互
2. -t 在新容器内指定一个伪终端或终端

### 启动容器（后台模式）

```shell
docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"

# 该命令的输出是一段哈希“574224410c255acae7ada2cc02f70a943e444aa0f59acb70d3c287628e583c6e”，这个长字符串叫做容器 ID，对每个容器来说都是唯一的，我们可以通过容器 ID 来查看对应的容器发生了什么。

#首先使用docker ps查看当前运行的容器
docker ps

CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS     NAMES
574224410c25   ubuntu:15.10   "/bin/sh -c 'while t…"   3 minutes ago   Up 3 minutes             cranky_saha
473b9a4bf792   ubuntu:15.10   "/bin/bash"              2 days ago      Up 2 days                compassionate_chaum

#CONTAINER ID: 容器 ID。

#IMAGE: 使用的镜像。

#COMMAND: 启动容器时运行的命令。

#CREATED: 容器的创建时间。

#STATUS: 容器状态。

#PORTS: 容器的端口信息和使用的连接类型（tcp\udp）。

#NAMES: 自动分配的容器名称。

```

容器的状态有7种：

1. created（已创建）
2. restarting（重启中）
3. running 或 Up（运行中）
4. removing（迁移中）
5. paused（暂停）
6. exited（停止）
7. dead（死亡）

使用两种方法查看容器内的输出：

```shell
docker logs 574224410c25
docker logs cranky_saha

docker stop 574224410c25 #停止该容器，也可以使用名字指定
```

> 插播小知识点 /bin/bash 与 /bin/sh的区别在于bash是一个更高级功能更多的shell，如果你想要你的docker更小启动更快，建议使用sh。

## 什么是docker？

Docker 容器是一个轻量级、可移植、自给自足的软件环境，用于运行应用程序。

Docker 容器将应用程序及其所有依赖项（包括库、配置文件、系统工具等）封装在一个标准化的包中，使得应用能够在任何地方一致地运行。

容器是操作系统级别的虚拟化，不需要运行完整的操作系统，启动和运行更为高效。

### 镜像与容器的关系
1. 镜像（Image）：容器的静态模板，包含了应用程序运行所需的所有依赖和文件。镜像是不可变的。
2. 容器（Container）：镜像的一个运行实例，具有自己的文件系统、进程、网络等，且是动态的。容器从镜像启动，并在运行时保持可变。

docker file ---> docker image ---> docker container

### 什么是docker的客户端与docker守护进程?
Docker 客户端是与 Docker 守护进程（Docker Daemon）交互的命令行工具。

docker 客户端非常简单，我们可以直接输入 docker 命令来查看到 Docker 客户端的所有命令选项。

解释docker的守护进程会有点复杂，这里举一个简单的例子：
1. 用户输入命令： docker run -d nginx
2. 客户端发送请求： 客户端将命令转换为http api请求，发送到守护程序监听的地址（默认是本地unix套接字/var/run/docker.sock）
3. 守护进程处理： 检查本地是否有nginx镜像，如果没有则拉取，创建容器并分配资源，启动容器后台运行
4. 返回结果： 守护程序将操作结果返回给客户端，客户端显示给用户

> Unix套接字:Unix 套接字（Unix Socket），也称为 Unix域套接字（Unix Domain Socket），是一种在同一台主机上的进程间进行高效通信的机制。它基于文件系统路径（而非网络协议）实现，专为本地进程间的数据传输设计，是 Unix/Linux 系统中常见的进程间通信（IPC, Inter-Process Communication）方式之一。

## 容器使用
如果本地没有镜像，可以使用`docker pull xxx`来获得镜像

使用`docker ps -a`查看所有容器
```shell
docker ps -a

CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS                     PORTS     NAMES
574224410c25   ubuntu:15.10   "/bin/sh -c 'while t…"   10 days ago   Exited (137) 10 days ago             cranky_saha
ff1d328e986e   ubuntu:15.10   "/bin/bash"              10 days ago   Exited (0) 10 days ago               funny_lovelace
d3f7ae6c4080   ubuntu:15.10   "/bin/bash"              10 days ago   Exited (127) 10 days ago             adoring_jemison
473b9a4bf792   ubuntu:15.10   "/bin/bash"              12 days ago   Exited (0) 10 days ago               compassionate_chaum
fb984360fcb0   ubuntu:15.10   "/bin/echo 'hello wo…"   12 days ago   Exited (0) 12 days ago               distracted_wiles
f3462ea03092   hello-world    "/hello"                 12 days ago   Exited (0) 12 days ago               admiring_robinson

# docker start 启动一个容器
root@VM-0-11-ubuntu:/home/july# docker start ff1d
ff1d

# 再次使用docker ps -a查看容器状态，发现ff1d已经是up状态
root@VM-0-11-ubuntu:/home/july# docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS                     PORTS     NAMES
574224410c25   ubuntu:15.10   "/bin/sh -c 'while t…"   10 days ago   Exited (137) 10 days ago             cranky_saha
ff1d328e986e   ubuntu:15.10   "/bin/bash"              10 days ago   Up 12 seconds                        funny_lovelace
d3f7ae6c4080   ubuntu:15.10   "/bin/bash"              10 days ago   Exited (127) 10 days ago             adoring_jemison
473b9a4bf792   ubuntu:15.10   "/bin/bash"              12 days ago   Exited (0) 10 days ago               compassionate_chaum
fb984360fcb0   ubuntu:15.10   "/bin/echo 'hello wo…"   12 days ago   Exited (0) 12 days ago               distracted_wiles
f3462ea03092   hello-world    "/hello"                 12 days ago   Exited (0) 12 days ago               admiring_robinson

#大部分情况下我们希望容器后台运行，加入-d参数，加入-d参数不会进入容器，如果需要进入，可以使用docker exec
root@VM-0-11-ubuntu:/home/july# docker run -itd --name test ubuntu:15.10 /bin/bash
95a22e6017c51a3538c6229291752b039be3d6530c82212fac38f5284b27fcad
root@VM-0-11-ubuntu:/home/july# docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                          PORTS     NAMES
95a22e6017c5   ubuntu:15.10   "/bin/bash"              7 seconds ago   Up 6 seconds                              test
574224410c25   ubuntu:15.10   "/bin/sh -c 'while t…"   10 days ago     Exited (137) 10 days ago                  cranky_saha
ff1d328e986e   ubuntu:15.10   "/bin/bash"              10 days ago     Exited (0) About a minute ago             funny_lovelace
d3f7ae6c4080   ubuntu:15.10   "/bin/bash"              10 days ago     Exited (127) 10 days ago                  adoring_jemison
473b9a4bf792   ubuntu:15.10   "/bin/bash"              12 days ago     Exited (0) 10 days ago                    compassionate_chaum
fb984360fcb0   ubuntu:15.10   "/bin/echo 'hello wo…"   12 days ago     Exited (0) 12 days ago                    distracted_wiles
f3462ea03092   hello-world    "/hello"                 12 days ago     Exited (0) 12 days ago                    admiring_robinson

#docker stop xxx 停止一个容器
docker stop 95a22

root@VM-0-11-ubuntu:/home/july# docker stop 95a22
95a22


# 如何进入一个容器 docker attach 或者 docker exec 建议使用docker exec ，因为docker attach进入容器后再退出会导致容器停止，而docker exec不会
docker ps -a
root@VM-0-11-ubuntu:/home/july# docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS                     PORTS     NAMES
95a22e6017c5   ubuntu:15.10   "/bin/bash"              22 hours ago   Up 2 seconds                         test
574224410c25   ubuntu:15.10   "/bin/sh -c 'while t…"   10 days ago    Exited (137) 10 days ago             cranky_saha
ff1d328e986e   ubuntu:15.10   "/bin/bash"              10 days ago    Exited (0) 22 hours ago              funny_lovelace
d3f7ae6c4080   ubuntu:15.10   "/bin/bash"              10 days ago    Exited (127) 10 days ago             adoring_jemison
473b9a4bf792   ubuntu:15.10   "/bin/bash"              13 days ago    Exited (0) 10 days ago               compassionate_chaum
fb984360fcb0   ubuntu:15.10   "/bin/echo 'hello wo…"   13 days ago    Exited (0) 13 days ago               distracted_wiles
f3462ea03092   hello-world    "/hello"                 13 days ago    Exited (0) 13 days ago               admiring_robinson


root@VM-0-11-ubuntu:/home/july# docker attach 95a2
root@95a22e6017c5:/# whoami
root
root@95a22e6017c5:/# exit
exit
root@VM-0-11-ubuntu:/home/july# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
root@VM-0-11-ubuntu:/home/july# docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS                     PORTS     NAMES
95a22e6017c5   ubuntu:15.10   "/bin/bash"              22 hours ago   Exited (0) 5 seconds ago             test
574224410c25   ubuntu:15.10   "/bin/sh -c 'while t…"   10 days ago    Exited (137) 10 days ago             cranky_saha
ff1d328e986e   ubuntu:15.10   "/bin/bash"              10 days ago    Exited (0) 22 hours ago              funny_lovelace
d3f7ae6c4080   ubuntu:15.10   "/bin/bash"              10 days ago    Exited (127) 10 days ago             adoring_jemison
473b9a4bf792   ubuntu:15.10   "/bin/bash"              13 days ago    Exited (0) 10 days ago               compassionate_chaum
fb984360fcb0   ubuntu:15.10   "/bin/echo 'hello wo…"   13 days ago    Exited (0) 13 days ago               distracted_wiles
f3462ea03092   hello-world    "/hello"                 13 days ago    Exited (0) 13 days ago               admiring_robinson

root@VM-0-11-ubuntu:/home/july# docker start 95a2
95a2
root@VM-0-11-ubuntu:/home/july# docker exec -it 95a2 /bin/bash
root@95a22e6017c5:/# whoami
root
root@95a22e6017c5:/# exit
exit
root@VM-0-11-ubuntu:/home/july# docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS                     PORTS     NAMES
95a22e6017c5   ubuntu:15.10   "/bin/bash"              22 hours ago   Up 21 seconds                        test
574224410c25   ubuntu:15.10   "/bin/sh -c 'while t…"   10 days ago    Exited (137) 10 days ago             cranky_saha
ff1d328e986e   ubuntu:15.10   "/bin/bash"              10 days ago    Exited (0) 22 hours ago              funny_lovelace
d3f7ae6c4080   ubuntu:15.10   "/bin/bash"              10 days ago    Exited (127) 10 days ago             adoring_jemison
473b9a4bf792   ubuntu:15.10   "/bin/bash"              13 days ago    Exited (0) 10 days ago               compassionate_chaum
fb984360fcb0   ubuntu:15.10   "/bin/echo 'hello wo…"   13 days ago    Exited (0) 13 days ago               distracted_wiles
f3462ea03092   hello-world    "/hello"                 13 days ago    Exited (0) 13 days ago               admiring_robinson


```

>docker run -itd xxx 与 docker start xxx的区别：Docker 中的 docker run -itd xxx 和 docker start xxx 是两个不同用途的命令，它们的核心区别在于 创建新容器和启动已有容器。docker run会创建并启动一个全新的容器（从image生成一个新的实例，而且你可以自定义名称），docker start只是用于启动

### 导入与导出容器
如果要导出本地某容器，可以使用`docker export`,可以使用`docker import`从容器快照文件再次导入为镜像。
```shell
root@VM-0-11-ubuntu:/home/july# docker export 95a2 > test.tar


cat docker/ubuntu.tar | docker import -test/ubuntu:v1
```

### 删除容器
使用`docker rm -f xxx`删除某个容器，另外`docker container prune`可以清理掉所有终止状态的容器。

```shell
root@VM-0-11-ubuntu:/home/july# docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS                      PORTS     NAMES
95a22e6017c5   ubuntu:15.10   "/bin/bash"              22 hours ago   Exited (0) 21 minutes ago             test
574224410c25   ubuntu:15.10   "/bin/sh -c 'while t…"   10 days ago    Exited (137) 10 days ago              cranky_saha
ff1d328e986e   ubuntu:15.10   "/bin/bash"              10 days ago    Exited (0) 22 hours ago               funny_lovelace
d3f7ae6c4080   ubuntu:15.10   "/bin/bash"              10 days ago    Exited (127) 10 days ago              adoring_jemison
473b9a4bf792   ubuntu:15.10   "/bin/bash"              13 days ago    Exited (0) 10 days ago                compassionate_chaum
fb984360fcb0   ubuntu:15.10   "/bin/echo 'hello wo…"   13 days ago    Exited (0) 13 days ago                distracted_wiles
f3462ea03092   hello-world    "/hello"                 13 days ago    Exited (0) 13 days ago                admiring_robinson
root@VM-0-11-ubuntu:/home/july# docker rm -f 95a2
95a2
root@VM-0-11-ubuntu:/home/july# docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS                     PORTS     NAMES
574224410c25   ubuntu:15.10   "/bin/sh -c 'while t…"   10 days ago   Exited (137) 10 days ago             cranky_saha
ff1d328e986e   ubuntu:15.10   "/bin/bash"              10 days ago   Exited (0) 22 hours ago              funny_lovelace
d3f7ae6c4080   ubuntu:15.10   "/bin/bash"              10 days ago   Exited (127) 10 days ago             adoring_jemison
473b9a4bf792   ubuntu:15.10   "/bin/bash"              13 days ago   Exited (0) 10 days ago               compassionate_chaum
fb984360fcb0   ubuntu:15.10   "/bin/echo 'hello wo…"   13 days ago   Exited (0) 13 days ago               distracted_wiles
f3462ea03092   hello-world    "/hello"                 13 days ago   Exited (0) 13 days ago               admiring_robinson
root@VM-0-11-ubuntu:/home/july# docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
574224410c255acae7ada2cc02f70a943e444aa0f59acb70d3c287628e583c6e
ff1d328e986eb234086d10649078b8aec82b4b895da9a503766f433daa6cb670
d3f7ae6c4080052a159f8fbef52074d145bf728daa2f6522f375ba676e2b957b
473b9a4bf792bde00d26db3c6060b638eaa0f80f66b55b1d85b2abd025920fb4
fb984360fcb054409125f5dbcd62281eb6b572edb1519cb10737a0efad21c451
f3462ea03092c2e8aa004d395692286b090c62b84b968a0f2486468a91c88707

Total reclaimed space: 40B
root@VM-0-11-ubuntu:/home/july# docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

### 运行一个web应用
```shell
docker pull nginx   #首先我们先拉取一个web应用程序的docker

docker run -d -P nginx  #这里的-d是后台运行，-P是随机映射端口

docker ps  # 查看目前运行的容器发现多了PORTS端口映射，容器内部的端口80被映射到了主机端口32768
root@VM-0-11-ubuntu:/home/july# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                       NAMES
013079f723c7   nginx     "/docker-entrypoint.…"   3 minutes ago   Up 3 minutes   0.0.0.0:32768->80/tcp, [::]:32768->80/tcp   objective_lehmann
```

接下来我们访问我们的服务，首先查看我们的ip

```shell
root@VM-0-11-ubuntu:/home/july# ifconfig
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::b8f0:c8ff:fe86:d90f  prefixlen 64  scopeid 0x20<link>
        ether ba:f0:c8:86:d9:0f  txqueuelen 0  (Ethernet)
        RX packets 15  bytes 420 (420.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 13  bytes 1406 (1.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.6.0.11  netmask 255.255.252.0  broadcast 10.6.3.255
        inet6 fe80::5054:ff:fe2b:8c8c  prefixlen 64  scopeid 0x20<link>
        ether 52:54:00:2b:8c:8c  txqueuelen 1000  (Ethernet)
        RX packets 12766456  bytes 3826312221 (3.8 GB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 10184128  bytes 1694251573 (1.6 GB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 1370567  bytes 139595582 (139.5 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1370567  bytes 139595582 (139.5 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethf9885d8: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::b4bf:6fff:fe07:899b  prefixlen 64  scopeid 0x20<link>
        ether 76:bf:03:bd:66:18  txqueuelen 0  (Ethernet)
        RX packets 3  bytes 126 (126.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 14  bytes 1156 (1.1 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

# 你会发现一个神奇的现象，这里面并没有我们的公网ip，这是为什么呢？其实这里是存在一个误解，ifconfig查看的是服务器内部的网路接口信息，而不是公网ip地址。公网ip不会直接显示在ifconfig的输出中，因为公网 IP 通常是通过 NAT（网络地址转换）映射到内网 IP 的,若想查看公网ip，你需要输入`curl ifconfig.me`


root@VM-0-11-ubuntu:/home/july# curl ifconfig.me
118.25.230.175root@VM-0-11-ubuntu:/home/july# 

# 这种网络架构是正常的，特别是在云服务器环境中：
# 服务器内部使用内网 IP（10.6.0.11）进行内部通信
# 外部访问通过公网 IP（118.25.230.175）进行
# 云服务提供商的网络设备负责在这两个地址之间进行转换
```

此时我们尝试访问`http://118.25.230.175:32768/`，无法访问，这是因为在云服务器环境中有安全组的配置，所以无法访问是正常的。

```shell
#docker logs xxx 查看容器内部的输出

root@VM-0-11-ubuntu:/home/july# docker logs 0130
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2025/06/05 07:48:57 [notice] 1#1: using the "epoll" event method
2025/06/05 07:48:57 [notice] 1#1: nginx/1.27.5
2025/06/05 07:48:57 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2025/06/05 07:48:57 [notice] 1#1: OS: Linux 6.8.0-51-generic
2025/06/05 07:48:57 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2025/06/05 07:48:57 [notice] 1#1: start worker processes
2025/06/05 07:48:57 [notice] 1#1: start worker process 29
2025/06/05 07:48:57 [notice] 1#1: start worker process 30
172.17.0.1 - - [05/Jun/2025:08:09:30 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/8.5.0" "-"
```


使用`docker top xxx`查看容器内部运行的进程

```shell
root@VM-0-11-ubuntu:/home/july# docker top 0130
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                2718760             2718735             0                   15:48               ?                   00:00:00            nginx: master process nginx -g daemon off;
message+            2718825             2718760             0                   15:48               ?                   00:00:00            nginx: worker process
message+            2718826             2718760             0                   15:48               ?                   00:00:00            nginx: worker process
```

