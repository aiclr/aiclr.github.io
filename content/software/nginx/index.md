+++
title = 'Nginx'
date = '2024-05-25'
draft = true
author = 'aiclr'
categories = ['software']
tags = ['nginx','docker','archlinux']
Summary='nginx (pronounced "engine X"), is a free, open-source, high-performance HTTP web server and reverse proxy, as well as an IMAP/POP3 proxy server, written by Igor Sysoev in 2005. nginx is well known for its stability, rich feature set, simple configuration, and low resource consumption.'
+++

[archlinux wiki](https://wiki.archlinux.org/title/Nginx) \
[the official documentation](https://nginx.org/en/docs/)

## install

Install one of the following packages:
- [nginx-mainline](https://archlinux.org/packages/?name=nginx-mainline) - mainline branch: new features, updates, bugfixes.
- [nginx](https://archlinux.org/packages/?name=nginx) - stable branch: major bugfixes only.
- [angieAUR](https://aur.archlinux.org/packages/angie/) - fork and drop-in replacement for nginx with more features.
- [freenginx-mainlineAUR](https://aur.archlinux.org/packages/freenginx-mainline/) - drop-in replacement that preserves the free and open development of nginx.

```sh
sudo pacman -S nginx
```

## running

```sh
# start
sudo systemctl start nginx.service
# if use Angie
sudo systemctl start angie.service

# status
sudo systemctl status nginx.service
# if use Angie
sudo systemctl status angie.service

# enable
sudo systemctl enable nginx.service
# if use Angie
sudo systemctl enable angie.service
```

## configuration

First steps with nginx are described in the [Beginner’s Guide](https://nginx.org/en/docs/beginners_guide.html). You can modify the configuration by editing the files in ***/etc/nginx/*** The main configuration file is located at ***/etc/nginx/nginx.conf***.

More details and examples can be found in [https://wiki.nginx.org/Configuration](https://wiki.nginx.org/Configuration) and the [official documentation](https://nginx.org/en/docs/).

The examples below cover the most common use cases. It is assumed that you use the default location for documents (***/usr/share/nginx/html***). If that is not the case, substitute your path instead.

Tip: A [Nginx configuration tool](https://www.digitalocean.com/community/tools/nginx/) has been provided by DigitalOcean.

## [configuration example](conf/nginx.conf)

```properties

#user http;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

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

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

    server {
        listen       8080;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   /usr/share/nginx/aiclr;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/aiclr;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```

## run nginx with docker

挂载外部配置文件时，注意先创建 `nginx.conf` \
可以先启动一个不挂载外部配置的容器，将容器内配置文件复制到宿主机编辑修改

```sh
docker run --name mynginx -d =p 8080:80 imageid

docker run --name mynginx -d -p8080:80 \
-v /root/nginx/html:/usr/share/nginx/html:ro \
-v /root/nginx/conf/nginx.conf:/etc/nginx/nginx.conf:ro \
-v /root/nginx/conf.d:/etc/nginx/conf.d:ro \
-v /root/nginx/logs:/var/log/nginx \ 
nginx:latest 
```

进入nginx容器: \
***alpine linux*** 默认使用 ***/bin/sh***
```sh
docker container exec -it containerid /bin/sh
```
其他发行版一般使用 ***/bin/bash***
```sh
docker container exec -it containerid /bin/bash
```
复制容器内配置文件 ***nginx.conf*** 到宿主机 ***/etc/nginx/docker/nginx.conf***
```sh
docker cp containerid:/etc/nginx/nginx.conf /etc/nginx/docker/nginx.conf
```

### nginx for vue and react

[nginx.conf](conf/docker/nginx.conf)

[Dockerfile](conf/docker/Dockerfile)

```dockerfile
FROM nginx:stable-alpine

MAINTAINER bougainvilleas <bougainvilleas@qq.com>

WORKDIR app

ENV TZ=Asia/Shanghai \
    LANG=en_US.UTF-8

COPY nginx.conf /etc/nginx/nginx.conf
# 将npm build 生成的前端包拷贝 /usr/share/nginx/html/ 这个目录下面
COPY dist  /usr/share/nginx/html/

RUN echo 'echo init ok!!'
```

[build.sh 脚本](conf/docker/build.sh)

```shell
#!/bin/bash

image_name="app"
version="0.0.0"
port="3000"


echo -e "\n==> begin delete all container of " $image_name
docker rm $(docker stop $(docker ps -a | grep $image_name | awk '{print $1}'))

echo -e "\n==> begin delete all images of " $image_name
docker rmi -f $(docker images | grep $image_name | awk '{print $3}')

echo -e "\n==> begin build your images of " $image_name
docker build -f Dockerfile -t $image_name:$version .

echo -e "\n==> begin to create a container of " $image_name
docker run -d -p $port:$port --name=$image_name --privileged=true $image_name:$version

docker logs -f -t --tail 500 $(docker ps | grep $image_name | awk '{print $1}')

docker exec -it $(docker container ls | grep $image_name | awk '{print $1}') /bin/sh

docker stop $(docker ps -a | grep $image_name | awk '{print $1}')
psid=$(docker ps -a | grep $image_name | awk '{print $1}')
if [[ -n $psid ]]; then
  docker rm $psid
fi

echo -e "\n==> begin to package your image to tar file"
docker save $image_name:$version > ../docker/$image_name-$version.tar

echo -e "\n==> begin load your images of " $image_name
docker load < $image_name-$version.tar
```