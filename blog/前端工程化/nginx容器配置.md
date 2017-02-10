# nginx容器配置

## 准备环境

- 安装docker的宿主机
- 获得最新nginx镜像 `docker pull nginx`
- 在数据盘上建立docker挂载目录nginx，分别建立子文件夹，`/var/nginx/conf`，`/var/nginx/content`

## 启动容器

```
docker run --name nginx -v /var/nginx/content:/usr/share/nginx/html:ro -v /var/nginx/conf:/etc/nginx/conf.d -d -p 80:80 nginx
```

这里需要注意的是，由于nginx的dockerfile将一个默认`nginx.vh.default.conf`写入到了`/etc/nginx/conf.d`中，这个配置文件主要用于设置静态资源文件夹。然而我们又将该路径作为了自定义配置文件的挂载路径，所以必须要在被挂载的路径中增加默认配置文件`server.conf`，配置内容如下：

```
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/log/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

}
```

如果没有事先增加该配置文件，需要重启容器`docker restart nginx`

在`/var/nginx/content`放置`index.html`文件，运行`curl http://localhost`，即可抓取到html内容。



