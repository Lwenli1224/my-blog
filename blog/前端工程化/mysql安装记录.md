# Mysql安装记录

## Docker下安装

```
docker run --name mysql -v /var/docker-apps/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=Passw0rd -d -p 3306:3306 mysql

```

## 安装准备

### 下载源码

社区版下载地址：http://dev.mysql.com/downloads/mysql/

本次安装版本为5.7，下载tar包并解压

### 安装编译

编译安装mysql需要cmake

```
yum install cmake
```

### 编译参数说明

-DCMAKE_INSTALL_PREFIX: 程序文件位置
-DMYSQL_DATADIR: 数据文件位置
-DSYSCONFDIR: 配置文件夹所在位置
-DTMPDIR: 临时文件夹所在位置
-DWITH_MYISAM_STORAGE_ENGINE=1 
-DWITH_INNOBASE_STORAGE_ENGINE=1 
-DWITH_MEMORY_STORAGE_ENGINE=1 
-DMYSQL_UNIX_ADDR=/var/tmp/mysql/mysql.sock 
-DMYSQL_TCP_PORT: mysql端口 
-DDEFAULT_CHARSET: 默认字符集
-DDEFAULT_COLLATION=utf8_general_ci
-DDOWNLOAD_BOOST: 下载加速库
-DWITH_BOOST: 加速库位置





### 执行编译安装

```
cmake -DCMAKE_INSTALL_PREFIX=/opt/mysql -DMYSQL_DATADIR=/var/mysql -DSYSCONFDIR=/etc -DTMPDIR=/var/tmp/mysql -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DMYSQL_UNIX_ADDR=/var/tmp/mysql/mysql.sock -DMYSQL_TCP_PORT=3306 -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/var/mysql/boost
```

### 配置环境变量







