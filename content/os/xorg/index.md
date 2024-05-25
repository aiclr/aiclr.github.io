+++
title = 'Xorg'
date = '2024-05-25'
draft = true
author = 'aiclr'
categories = ['os']
tags = ['xorg','archlinux']
Summary='Xorg (commonly referred to as simply X) is the most popular display server among Linux users.'
+++

## startx

Linux提供虚拟控制台的功能，一组终端设备共享PC电脑的屏幕、键盘和鼠标。 \
通常一个Linux安装配置8个或者12个虚拟控制台。虚拟控制台通过字符设备文件 ***/dev/ttyN*** 使用，其中 ***N*** 代表一个数字，从1开始 \
使用字符界面登陆Linux系统，所使用的终端设备就是系统中的第一个虚拟控制台，即终端设备 ***/dev/tty1***。 \
使用 ***Ctrl+Alt+F<N>*** 在不同的虚拟控制台之间切换，其中 ***N*** 是虚拟控制台 ***/dev/ttyN*** 所对应的数字 \
当在字符界面而不是图形界面进行虚拟控制台的切换时，需要使用组合键 ***Alt+F<N>*** \
默认情况，X视窗系统从 ***session:0*** 启动。要在Linux系统上运行多个X视窗会话必须告诉 ***startX*** 启动 ***session:1***。 \
命令 ***startx -- :1***, Linux 将在下一个未使用的虚拟控制台上启动X服务器 \
在一个Xsession中启动gkrellm，并让其在另一个Xsession中显示 ***DISPLAY=:1 gkrellm &*** \
KDE 的 ***SDDM*** 和 GNOME 的 ***GDM*** 是显示管理器<sub>display manager</sub>，默认使用第一个未使用的虚拟控制台，通常是 ***/dev/tty7***。
