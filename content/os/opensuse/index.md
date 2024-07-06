+++
title = 'Opensuse'
date = '2024-07-06'
author = 'aiclr'
categories = ['os']
tags = ['opensuse']
Summary='The openSUSE project is a worldwide effort that promotes the use of Linux everywhere. '
+++

## init

旧电脑启动后需要 `systemctl restart wicked` 才可以ping 通

### opensuse 启动后执行脚本

```shell
# micro os 测试可行
mkdir -p /etc/init.d
touch /etc/init.d/after.local
vim /etc/init.d/after.local
chmod +x /etc/init.d/after.local
reboot
```

#### after.local

```shell
#!/usr/bin/sh
/usr/bin/systemctl restart wicked
/usr/bin/echo "测试是否执行脚本" > /root/test.log
```

---

## 原子更新

```shell
transactional-update shell
zypper ref #刷新
zypper dup #更新
exit
transactional-update cleanup #清理未使用的系统快照
```

## zypper

### repo

```shell
Repository Management:
  repos, lr             List all defined repositories.
  addrepo, ar           Add a new repository.
  removerepo, rr        Remove specified repository.
  renamerepo, nr        Rename specified repository.
  modifyrepo, mr        Modify specified repository.
  refresh, ref          Refresh all repositories.
  clean, cc             Clean local caches.
# 例子
zypper repos -d # 查看源详细信息
zypper mr -da # 禁用全部源
# 添加国内源
zypper addrepo -f https://mirrors.bfsu.edu.cn/opensuse/tumbleweed/repo/non-oss bfsu-non-oss
zypper addrepo -f https://mirrors.bfsu.edu.cn/opensuse/tumbleweed/repo/oss/ bfsu-oss
zypper ar -f https://mirrors.bfsu.edu.cn/opensuse/tumbleweed/repo/non-oss bfsu-non-oss
zypper ar -f https://mirrors.bfsu.edu.cn/opensuse/tumbleweed/repo/oss/ bfsu-oss
 
zypper refresh # 刷新
zypper ref # 刷新
zypper dup # 更新
```

### package

```shell
zypper in docker # 安装docker

master:~/parking # cat /etc/docker/daemon.json
{
  "registry-mirrors":["https://rjm3pmfv.mirror.aliyuncs.com"]
}
zypper in docker-compose # 安装docker-compose
zypper in htop # htop
zypper in tmux # tmux
zypper in lrzsz # lrzsz
zypper in zsh # zsh
zypper in git # git
# 安装 ohmyzsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

zypper in podman # 安装 podman
zypper in nfs-kernel-server # 安装 NFS
```

---

## opensuse java 管理工具 `alternatives --config java`

### example

```shell
caddy@localhost:~> java -version
java version "1.8.0_321"
Java(TM) SE Runtime Environment (build 1.8.0_321-b07)
Java HotSpot(TM) 64-Bit Server VM (build 25.321-b07, mixed mode)
caddy@localhost:~> sudo alternatives --config java
[sudo] root 的密码：
There are 3 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                    Priority   Status
------------------------------------------------------------
  0            /usr/java/jdk-17.0.2/bin/java            170002000 auto mode
  1            /usr/java/jdk-17.0.2/bin/java            170002000 manual mode
* 2            /usr/java/jdk1.8.0_321-amd64/bin/java    180321    manual mode
  3            /usr/lib64/jvm/jre-11-openjdk/bin/java   2105      manual mode

Press <enter> to keep the current choice[*], or type selection number: 1
update-alternatives: using /usr/java/jdk-17.0.2/bin/java to provide /usr/bin/java (java) in manual mode
caddy@localhost:~> java -version
java version "17.0.2" 2022-01-18 LTS
Java(TM) SE Runtime Environment (build 17.0.2+8-LTS-86)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.2+8-LTS-86, mixed mode, sharing)
```

---

## Micro OS

### 原子更新系统 [Micro OS](https://en.opensuse.org/Portal:MicroOS)

[国内外国语大学开源软件镜像站 iso 下载地址](https://mirrors.bfsu.edu.cn/opensuse/tumbleweed/iso/openSUSE-MicroOS-DVD-x86_64-Current.iso)

### 原子更新系统 [Kubic](https://en.opensuse.org/Portal:MicroOS)

[国内外国语大学开源软件镜像站 iso 下载地址](https://mirrors.bfsu.edu.cn/opensuse/tumbleweed/iso/openSUSE-Kubic-DVD-x86_64-Current.iso)

---

## nfs

> [opensuse Kubic](https://en.opensuse.org/SDB:Network_file_system) \
> [archlinux wiki](https://wiki.archlinux.org/title/NFS)
> 
> 挂载网络上的文件系统
> 
> Install NFS kernel server `zypper in nfs-kernel-server` \
> `systemctl status nfs-server.service` \
> `systemctl start nfs-server.service` \
> `systemctl enable nfs-server.service`


### server configure /etc/exports

```
# This shares the directory read-only to ALL hosts
/root/export/this/directory   (ro)
# This shares the directory read-and-write to all hosts in the 192.168.1.0 network, which subnet mask 255.255.255.0
/root/export/this/directory   192.168.1.0/24(rw,async)
# 当客户端使用root用户时无法需要在服务端添加此配置 no_root_squash;默认 root_squash
/root/export/this/directory 192.168.10.0/24(rw,no_root_squash,async)
```

### client configure /etc/fstab

```properties
192.168.1.2:/root/export/this/directory  /local/mountpoint   nfs   auto    0   0
```

---

## rebootmg

- [opensuse wiki](https://en.opensuse.org/Kubic:Update_and_Reboot)
- [rebootmgrctl](https://kubic.opensuse.org/documentation/man-pages/rebootmgrctl.1.html) 

```sh
# Disable Automatic Updates
systemctl --now disable transactional-update.timer

# Disable Rebootmgr
systemctl --now disable rebootmgr
## or
rebootmgrctl set-strategy off
```

### /etc/rebootmgr.conf A default configuration file would be:

```text
[rebootmgr]
window-start=03:30
window-duration=1h30m
strategy=best-effort
lock-group=default
```