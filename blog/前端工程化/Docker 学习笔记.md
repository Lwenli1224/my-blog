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

在OS上我们并不能直接使用Docker而是要通过Docker Machine新建出一个最基本的虚拟机，真正的Docker是运行在该linux虚机中，而OS中的Docker仅仅是一个和虚机对话的入口。

使用`docker-machine create`命令新建虚机

```shell
docker-machine create --driver virtualbox default

eval $(docker-machine env dev)
```

开启虚机后，即可在本地终端使用docker

```shell
docker-machine start default
```

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

设定容器启动时需要执行的命令，可被`docker run`指定运行的命令覆盖。

#### ENTRYPOINT

同样为容器启动时执行的命令，与`CMD`命令不同的是，它不能被`docker run`指定的命令所覆盖，而是会与外部所指定的命令形成新的命令。可以`CMD`命令构造出默认命令参数。

#### WORKDIR

#### USER

#### RUN

#### VOLUME

向基于镜像创建的容器添加卷。卷用于存放代码、数据库，而不是将这些内容提交到镜像之中。

#### ADD

将文件提交到镜像中，文件必须处于构建目录下，也就是说必须与*Dockerfile*文件处于同一文件夹下。

```shell
ADD ./nginx.conf /etc/nginx/nginx.conf
```

#### EXPOSE

设置该容器对外暴露的端口


### 构建镜像


## 运行容器

### 卷

卷在搭建docker工作栈中的地位非常重要，我们的代码和数据库文件一般都是放在卷中，以挂载的方式供容器使用。因为，docker容器的特性，如果该容器因为某些原因被销毁，或者损坏无法进入，就会导致该容器保存的所有磁盘文件丢失，所以我们一定要把数据保存在本地宿主机的数据盘上。这样，即使docker容器销毁，它所生成或修改的数据仍然存在，我们只需要重新启动一个相同服务的容器，将数据所在文件夹挂载至其上，即可以继续提供服务。

可以通过以下两种方式使用docker的卷功能

- 在

### 容器

### 网络

## 进阶使用

### 备份






