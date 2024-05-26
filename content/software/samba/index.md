+++
title = 'Samba'
date = '2024-05-25'
draft = true
author = 'aiclr'
categories = ['software']
tags = ['samba','archlinux']
Summary='Samba is the standard Windows interoperability suite of programs for Linux and Unix. Since 1992, Samba has provided secure, stable and fast file and print services for all clients using the SMB/CIFS protocol, such as all versions of DOS and Windows, OS/2, Linux and many others.'
+++

[参考arch wiki](https://wiki.archlinux.org/title/Samba)

注意：优先配置 配置文件 ***/etc/samba/smb.conf*** \
配置文件官方样例 [smb.conf.default](https://git.samba.org/samba.git/?p=samba.git;a=blob_plain;f=examples/smb.conf.default;hb=HEAD) \
***smb.conf*** [文档说明](https://man.archlinux.org/man/smb.conf.5) \
自用配置文件 [smb.conf](conf/smb.conf)

配置完毕 进行安装:
```sh
pacman -S samba
# 创建 共享专用账户 
useradd -m hi
# 禁止登录 禁止 ssh login 
usermod --shell /usr/bin/nologin --lock hi
# samba 拥有自己的密码管理
smbpasswd -a hi
# 列出 samba 用户
pdbedit -L -v
# 启动测试
systemctl start smb
# 开机自启动
systemctl enable smb
```

### Linux 客户端挂载 samba

```sh
sudo mount //192.168.1.165/work /mnt -o username=fly,password=fly,iocharset=utf8
```
