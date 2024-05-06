+++
title = 'Journal'
date = '2024-05-05'
draft = true
author = 'aiclr'
categories = ['tools']
tags = ['tools','systemd','archlinux']
+++

[archlinux Wiki](https://wiki.archlinux.org/title/Systemd/Journal)
[opensuse man](https://www.freedesktop.org/software/systemd/man/journalctl.html#)

help: ***journalctl -h --help***

检索 *pacman* 相关日志 ***journalctl --grep=pacman***

Show only the most recent journal entries, and continuously print new entries as they are appended to the journal.\
显示最新日志，并连续输出 ***journalctl -f --follow***

Reverse output so that the newest entries are displayed first\
反转输出 以便先显示最新的日志 ***journalctl -r --reverse***

Show all kernel logs from previous boot\
显示上次启动的所有内核日志 ***journalctl -k -b -1***

Show a live log display from a system service *kubelet.service*\
实时显示 *kubelet* 日志 ***journalctl -f -u kubelet***

查看 *kubelet* 日志 ***journalctl _SYSTEMD_UNIT=kubelet.service***

查看进程号 *28097* 日志 ***journalctl _PID=28097***

查看进程号 *28097* 的 kubelet服务的日志 ***交集***\
***journalctl _SYSTEMD_UNIT=kubelet.service _PID=28097***