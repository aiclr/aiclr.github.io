+++
title = 'Systemd'
date = '2024-05-25'
draft = true
author = 'aiclr'
categories = ['tools']
tags = ['systemd','archlinux']
Summary='systemd is a suite of basic building blocks for a Linux system. It provides a system and service manager that runs as PID 1 and starts the rest of the system. '
+++

[archlinux Wiki](https://wiki.archlinux.org/title/Systemd) \
[i3wm 普通用户关机 重新安装 polkit 即可](https://wiki.archlinux.org/title/Allow_users_to_shutdown)

### root

- ***systemctl*** = ***systemctl --system***
- ***systemctl enable NetworkManager*** = ***systemctl --system enable NetworkManager***
- ***systemctl enable sshd*** = ***systemctl enable sshd --system***
- ***systemctl enable update-system.timer*** = ***systemctl --system enable update-system.timer***

poweroff
```sh
systemctl poweroff
```
reboot
```sh
systemctl reboot
```
Show system status
```sh
systemctl status
```
List running units
```sh
systemctl
systemctl list-units
```
List failed units
```sh
systemctl --failed
```
List installed unit files
```sh
systemctl list-unit-files
```

### not root

参考[archlinux wiki](https://wiki.archlinux.org/title/Systemd/User) \
offers the ability to manage services under the user's control with a per-user systemd instance,enabling them to start, stop, enable, and disable their own user units \
***systemctl --user*** Connect to user service manager
- ***systemctl --user enable mpd***
- ***systemctl --user start mpd***


### systemd/Timers

There are many cron implementations, but none of them are installed by default as the base system uses **systemd/Timers** instead. \
[archlinux Wiki](https://wiki.archlinux.org/title/Systemd/Timers) \
[systemd.timer](https://man.archlinux.org/man/systemd.timer.5) \
[systemd.time 执行时间策略](https://man.archlinux.org/man/systemd.time.7) \ 
[systemd.exec](https://man.archlinux.org/man/systemd.exec.5)

file path
- /etc/systemd/system/update-system.timer
- /etc/systemd/system/update-system.service

update-system.timer
```properties
[Unit]
Description=update system weekly

[Timer]
OnCalendar= Fri *-*-* 12:00:00

[Install]
WantedBy=timers.target
```

update-system.service
```properties
[Unit]
Description=update system weekly

[Service]
ExecStart=pacman -S -y --noconfirm -u
```

查看定时任务
```sh
systemctl list-timers
systemctl list-timers --all
```
开机激活定时器
```sh
systemctl enable update-system.timer
```
取消开机激活定时器
```sh
systemctl disable update-system.timer
```
手动激活定时器
```sh
systemctl start update-system.timer
```
手动停止定时器
```sh
systemctl stop update-system.timer
```
查看定时器状态
```sh
systemctl status update-system.timer
```

### OnCalendar

```sh
systemd-analyze calendar weekly
systemd-analyze calendar "Mon *-*-* 00:00:00"
```
注意 ***/*** 仅用于月份

| desc                |       word       |        周 年-月-日 时:分:秒        |
|:--------------------|:----------------:|:---------------------------:|
| 每分钟                 |     minutely     |       `*-*-* *:*:00`        |
| 从0分钟起每5分钟           |     minutely     |      `*-*-* *:00/5:00`      |
| 每小时                 |      hourly      |       `*-*-* *:00:00`       |
| 每天                  |      daily       |      `*-*-* 00:00:00`       |
| 每月                  |     monthly      |      `*-*-01 00:00:00`      |
| 每周                  |      weekly      |    `Mon *-*-* 00:00:00`     |
| 每年                  | yearly /annually |     `*-01-01 00:00:00`      |
| 每季度                 |    quarterly     | `*-01,04,07,10-01 00:00:00` |
| 每半年                 |   semiannually   |    `*-01,07-01 00:00:00`    |
| 九月倒数第一天0点           |                  |     `*-09~01 00:00:00`      |
| 每年的九月倒数第七天起每过4天后的周一 |                  |  `Mon *-09~07/4 00:00:00`   |
| 下一个周一或周五            |                  |  `Mon,Fri *-*-* 00:00:00`   |
| 周一到周五每天0点           |                  |  `Mon..Fri *-*-* 00:00:00`  |
