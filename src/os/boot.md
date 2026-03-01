# 启动问题排查

开机启动树

```sh
systemd-cgls
```

开机耗时总览

```sh
systemd-analyze
```

开机耗时详情

```sh
systemd-analyze blame
```

开机启动依赖项

```sh
systemd-analyze critical-chain
```

### problem

小米游戏本开机变慢发现是 **_systemd-journal-flush.service_** 耗时30秒+

查看日志文件大小40M

```sh
journalctl -b -u systemd-journald
```

查看启动耗时

```sh
journalctl -b -u systemd-journal-flush.service
```
