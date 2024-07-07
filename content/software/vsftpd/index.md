+++
title = 'Vsftpd'
date = '2024-07-06'
author = 'aiclr'
categories = ['software']
tags = ['vsftpd','linux']
Summary='vsftpd (Very Secure FTP Daemon) is a lightweight, stable and secure FTP server for UNIX-like systems.'
+++

vsftpd<sub>Very Secure FTP Daemon</sub> 文件服务器 Centos7 搭建

## install

```shell
yum -y install vsftpd
systemctl enable vsftpd
```

## config

#### 不允许匿名登录

```shell
vi /etc/vsftpd/vsftpd.conf
```

```properties
anonymous_enable=NO
```

#### 创建ftp用户

```shell
useradd ftpuser
passwd password
```

#### 禁用ssh

不允许用户 **ssh** 登录系统，但是能登陆 **vsftpd**

```shell
# 将which nologin 显示的内容加到shells文件末尾
which nologin
vi /etc/shells
usermod -s /usr/sbin/nologin ftpuser

# 测试
su -ftpuser
systemctl restart vsfypd
```

#### 开放防火墙策略

```shell
firewall-cmd --zone=public --add-service=ftp --permanent
firewall-cmd --reload:q
```

#### 修改sshd配置

```shell
vi /etc/ssh/sshd_config

LoginGraceTime 0
MaxSessions 1000
Subsystem sftp internal-sftp
Match group ftpuser
ChrootDirectory /home/ftpuser
X11Forwarding no
AllowTcpForwarding no
ForceCommand internal-sftp
ClientAliveInterval 60
ClientAliveCountMax 10
```

#### 权限设置

```shell
# ChrootDirectory设置的目录权限及其所有上级文件夹权限，属主和属组必须是root
chown root:root /home/ftpuser
# ChrootDirectory只有属主能拥有写权限，权限最大设置只能是755
chmod 755 /home/ftpuser
```

## 重启

修改配置后要重启 **sshd** 和 **vsftpd** 服务

```shell script
systemctl restart sshd
systemctl restart vsftpd

service sshd restart
service restart vsftpd
```