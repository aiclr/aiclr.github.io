+++
title = 'i3'
date = '2024-07-07'
author = 'aiclr'
categories = ['software']
tags = ['i3','wm','linux']
Summary='i3 is a dynamic tiling window manager inspired by wmii that is primarily targeted at developers and advanced users.'
+++

[archlinux wiki](https://wiki.archlinux.org/title/I3)


```shell
pacman -S i3-gaps i3lock i3status feh picom scrot terminator
```

## 配置文件

- 全局配置  
    - `/etc/i3/config`
    - `/etc/i3status.conf`
- 用户配置
    - [~/.config/i3/conf](conf/config)
    - [~/.config/i3/i3status.conf](conf/i3status.conf)
    - <https://github.com/levinit/i3wm-config>

### 鼠标光标大小

- [~/.Xresources](conf/.Xresources)
    - Xcursor.size= 12


## package 

#### i3-gaps(i3-wm的子分支，支持更多配置)配置

```shell
gaps inner 3
gaps outer 3 
smart_gaps on

new_window none
new_float normal
hide_edge_borders both
```

#### i3-lock 锁屏

```shell
# i3lock image must be PNG, just save as PNG ,not mv png to PNG
mode "i3lock: Return to lock/Escape to Cancel" {
bindsym Return mode "default" exec i3lock -I 600 -i /home/caddy/Pictures/bg/lock.PNG
bindsym Escape mode "default"
}
bindsym Ctrl+mod1+l mode "i3lock: Return to lock/Escape to Cancel"
```

#### i3-status 

bar status_command

```sh
i3status -c ~/.config/i3/i3status.conf
```

#### 壁纸 feh

```sh
exec --no-startup-id feh --bg-fill ~/Pictures/bg/d469095e66520d0a42c0b9566b81d65c431b935d.jpg
```

#### 窗口透明 picom

```sh
exec --no-startup-id picom
```

#### 截图 scrot

```shell
pacman -S scrot
# printscreen--scrot(Print=PrtSc/PrintScreen)
bindsym $mod+Print exec --no-startup-id scrot
```

#### terminator

[archlinux wiki](https://wiki.archlinux.org/title/Terminator)

#### conky

[archlinux wiki](https://wiki.archlinux.org/title/Conky)
  