+++
title = 'Docker'
date = '2024-05-25'
draft = true
author = 'aiclr'
categories = ['software']
tags = ['docker','archlinux']
Summary='Docker is a utility to pack, ship and run any application as a lightweight container.'
+++

[Guides](https://docs.docker.com/get-started/overview/) \
[Reference](https://docs.docker.com/reference/)

### 局域网访问

开启远程 
```sh
vi /lib/systemd/system/docker.service
```
修改配置
```properties
[Service]
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
```
重启docker 
```sh
systemctl daemon-reload
systemctl restart docker
```
***firewall-cmd*** 开启端口
```sh
firewall-cmd --zone=public --add-port=2375/tcp --permanent
```
