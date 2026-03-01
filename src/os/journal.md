# Journal

systemd has its own logging system called the journal; running a separate logging daemon is not required. To read the log, use journalctl(1).

- [archlinux Wiki](https://wiki.archlinux.org/title/Systemd/Journal)
- [opensuse man](https://www.freedesktop.org/software/systemd/man/journalctl.html#)

help:

```shell
journalctl -h --help
```

检索 _pacman_ 相关日志

```shell
journalctl --grep=pacman
```

Show only the most recent journal entries, and continuously print new entries as they are appended to the journal.\
显示最新日志，并连续输出

```shell
journalctl -f --follow
```

Reverse output so that the newest entries are displayed first\
反转输出 以便先显示最新的日志

```shell
journalctl -r --reverse
```

Show all kernel logs from previous boot\
显示上次启动的所有内核日志

```shell
journalctl -k -b -1
```

Show a live log display from a system service _kubelet.service_\
实时显示 _kubelet_ 日志

```shell
journalctl -f -u kubelet
```

查看 _kubelet_ 日志

```shell
journalctl _SYSTEMD_UNIT=kubelet.service
```

查看进程号 _28097_ 日志

```shell
journalctl _PID=28097
```

查看进程号 _28097_ 的 kubelet服务的日志 **_交集_**

```shell
journalctl _SYSTEMD_UNIT=kubelet.service _PID=28097
```
