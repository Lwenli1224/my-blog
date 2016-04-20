# Docker 学习笔记

## 概念介绍

### 某某即服务

#### IaaS

Infrastructure-as-a-Service（基础设施即服务）
 
厂商提供硬件设备的租赁
 
 
#### PaaS

Platform-as-a-Service（平台即服务）

也称为中间件，厂商提供虚拟服务器和操作系统
	
 
#### SaaS

Software-as-a-Service（软件即服务）

厂商提供应用服务

### Docker术语

#### Container


### Docker体系

- Docker Machine：帮助用户快速安装docker主机，并接受来自docker客户端的命令，执行一系列docker相关操作
- Docker Engine：
- Docker Compose：


## 安装准备

笔者是在mac上运行docker

安装Docker需要Docker Toolbox，ToolBox包括：
	
- Docker Machine
- Docker Engine
- Docker Compose
- Kitematic
- Oracle VM VirtualBox

前往https://www.docker.com/docker-toolbox下载Docker Toolbox

直接执行安装文件

## Get Started

在OS上我们并不能直接使用Docker而是要通过Docker Machine新建出一个最基本的虚拟机，然后进入该虚机建立docker镜像

使用`docker-machine create`命令新建虚机

```shell
docker-machine create --driver virtualbox default
```

使用`docker-machine ssh`登录虚机

## 构建镜像

### 镜像

Docker利用了Linux中的联合文件系统来定义镜像，镜像是由多层文件系统叠加到一起形成的。

Docker镜像的第一层是引导文件系统，即bootfs当容器启动后，该系统会被卸载，以节省更多内存。第二层rootfs，在Docker中root文件系统永远只是只读状态。Docker还会利用联合加载在rootfs加载更多只读文件系统。当从镜像启动一个容器时，Docker会在镜像最顶端增加一个读写文件系统，容器中的程序就是在这个系统里执行的。

在第一次启动容器时，初始的读写层是空的，当文件系统发生变化后，所有的变化都会应用在这一层上。要注意的是，当我们想要修改一个文件时，Docker会先从该读写层下方的只读层复制到该读写层，原只读版本仍存在。这种机制被称为写时复制(COW)。

### Dockfile指令

#### FROM

指定基础镜像，后续指令都将基于该镜像进行

#### MAINTAINER

标明镜像作者以及作者的电子邮件地址

#### ENV

设定环境变量，可设定一个当前时间，用于刷新镜像缓存

#### CMD

#### ENTRYPOINT

#### WORKDIR

#### USER

#### RUN

#### VOLUME

#### ADD

#### EXPOSE

####

## 运行容器

### 端口




