# Ghost Docker 部署

## 配置mysql

## Ghost部署

### 拉取镜像

首先拉取官方镜像latest：

```
docker pull ghost
```

### 创建配置文件

### 运行镜像

```
docker run --name my-ghost -d -p 2368:2368 -v /home/ghost/content:/var/lib/ghost -e "NODE_ENV=production"
```





