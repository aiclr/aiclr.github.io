+++
title = 'Pacman'
date = '2024-05-05'
author = 'aiclr'
categories = ['tools']
tags = ['tools','archlinux']
Summary = "The pacman package manager is one of the major distinguishing features of Arch Linux. It combines a simple binary package format with an easy-to-use build system. The goal of pacman is to make it possible to easily manage packages, whether they are from the official repositories or the user's own builds."
+++

## config

下载的包本地保存目录 */var/cache/pacman/pkg/*

**mirrorlist** */etc/pacman.d/mirrorlist*

[配置文件 */etc/pacman.conf*](conf/pacman.conf)

更新时忽略包 ***vim /etc/pacman.conf*** 进行如下配置

```properties
# Pacman won't upgrade packages listed in IgnorePkg and members of IgnoreGroup
IgnorePkg = intellij-idea-ultimate-edition intellij-idea-ultimate-edition-jre
```


## commands

帮助

```shell
pacman -h
pacman -Sh
```

跳过手动输入 **Y/N** 确认参数 *--noconfirm*

更新系统 
```shell
pacman -Syu
```

**慎用** 强制更新系统 
```shell
pacman -Syy
```

查询 
```shell
pacman -Ss package_name
```

安装
```shell
pacman -S package_name
```

下载不安装
```shell
pacman -Sw package_name
```

删除单个软件包，保留其全部已经安装的依赖关系
```shell
pacman -R package_name
```

删除指定软件包，及其所有没有被其他已安装软件包使用的依赖关系
```shell
pacman -Rs package_name
```

删除软件包和所有依赖这个软件包的程序 警告: 此操作是递归的，请小心检查，可能会一次删除大量的软件包
```shell
pacman -Rsc package_name
```

删除软件包 *-d* 跳过依赖版本检查 *-dd* 跳过所有检查 
```shell
pacman -Rdd package_name
```

**pacman** 删除某些程序时会备份重要配置文件，在其后面加上 `*.pacsave` 扩展名。*-n* 选项可以删除这些文件
```shell
pacman -Rn package_name
pacman -Rsn package_name
```

从 */var/cache/pacman/pkg/* 本地包缓存目录中删除旧包 (*-cc* 删除全部包)
```shell
pacman -Sc
pacman -Scc
```

## 降级

不建议乱来，可以先在虚拟机上实验 [参考](https://wiki.archlinux.org/title/Downgrading_packages) 
```shell
pacman -U xxx
```

低版本包检索地址 [archlinux packages](https://archive.archlinux.org/packages/) 

Install a **local** package
```shell
pacman -U /var/cache/pacman/pkg/xxx.pkg.tar.zst
```

Install a **remote** package 
```shell
pacman -U https://archive.archlinux.org/packages/f/ffmpeg/ffmpeg-2%3A4.4.1-1-x86_64.pkg.tar.zst
```

## AUR

需要安装 *base-devel、git*
```shell
pacman -S base-devel git
```

[搜索 AUR 包](https://aur.archlinux.org/packages) 

使用 *git* 下载AUR包
```shell
git clone xxx.git
```

安装命令
```shell
makepkg -sirc
```
To build the package and install needed dependencies, add the flag
```shell
makepkg -s/--syncdeps
```

To clean up leftover files and directories
```shell
makepkg -c/--clean
```

makepkg to remove the make dependencies later, which are no longer needed
```shell
makepkg -r/--rmdeps
```

Once all dependencies are satisfied and the package builds successfully,a package file (***pkgname-pkgver.pkg.tar.zst***) will be created in the working directory.

To install, use *-i/--install*
```shell
makepkg -i/--install
# same as
pacman -U pkgname-pkgver.pkg.tar.zst
```

example:
```shell
git clone xxx.git
cd xxx
makepkg -sirc
```