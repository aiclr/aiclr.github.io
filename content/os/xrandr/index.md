+++
title = 'Xrandr'
date = '2024-05-25'
author = 'aiclr'
categories = ['os']
tags = ['xrandr','archlinux']
Summary='xrandr is an official configuration utility to the RandR (Resize and Rotate) X Window System extension. It can be used to set the size, orientation or reflection of the outputs for a screen. '
+++

[archlinux-wiki 多显示器](https://wiki.archlinux.org/title/Xrandr)\
[archlinux-wiki 显示器亮度](https://wiki.archlinux.org/title/Backlight#Xorg:_adjust_perceived_brightness_with_xrandr)\
安装
```sh
pacman -S xorg-xrandr
```

### example

帮助
```sh
xrandr --help
```
列出信息
```sh
xrandr
```
列出激活的监视器
```sh
xrandr --listactivemonitors
```
列出监视器
```sh
xrandr --listmonitors
```
列出显卡输出源
```sh
xrandr --listproviders
```
分辨率+刷新率
```sh
xrandr --output eDP1 \
--brightness 0.77 \
--mode 1920x1080 \
--rate 60 \
--primary --auto \
--output "$extern" \
--left-of "$intern" --auto
```

## 小米游戏本 

***type-c*** 接口名称 ***DP1***,可以正常双屏输出信号\
***hdmi*** 接口名称 ***HDMI-1***, 独立显卡 ***GTX1060 Mobile***,驱动 ***nvidia*** 显示器无法检测到信号\
***fn*** 键基本全部失效

### 简单脚本

```shell
#!/bin/zsh
intern=eDP1
extern=DP1
# 注意亮度值`0.00~1.00`
if xrandr | grep "$extern disconnected"; then
	xrandr --output "$extern" --off --output "$intern" --brightness 0.77 --auto
else
	xrandr --output "$intern" --brightness 0.77 --primary --auto --output "$extern" --left-of "$intern" --auto
fi
```