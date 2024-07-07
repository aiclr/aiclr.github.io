+++
title = 'MPD'
date = '2024-07-07'
author = 'aiclr'
categories = ['software']
tags = ['mpd','linux']
Summary='MPD (music player daemon) is an audio player that has a server-client architecture. It plays audio files, organizes playlists and maintains a music database, all while using very few resources. In order to interface with it, a separate client is needed.'
+++

### mpv 服务端

- [MPD Arch wiki](https://wiki.archlinux.org/title/Music_Player_Daemon)
- [mpc Arch wiki](https://wiki.archlinux.org/title/Music_Player_Daemon#Command-line)
- [mpd.conf](conf/mpd.conf)

```shell
# mpd server
# mpc client
pacman -S mpd mpc

# Configuration source
mkdir ~/.config/mpd
mkdir ~/.config/mpd/playlists
cp /usr/share/doc/mpd/mpdconf.example ~/.config/mpd/mpd.conf

# not root
# --user Connect to user service manager
systemctl --user enable mpd
systemctl --user start mpd

# mpc 客户端状态
mpc status

# 刷新 mpd 数据库
mpc update

# 查看所有歌曲
mpc listall

# mpc add <tab> 添加 歌曲 或 文件夹 到 播放列表
mpc add <tab>

# 播放
mpc play

# 音量 0-100
mpc volume 50

# repeat 循环开关
mpc repeat

# random 随即开关
mpc random

# single 单曲开关
mpc single

```

### mpd.conf

```text
# 歌曲保持目录 
music_directory		"~/samba/audio/music"
# 播放列表
playlist_directory		"~/.config/mpd/playlists"
# mpc update 后根据歌曲目录自动生成的数据库
db_file			"~/.config/mpd/database"
# systemd journal 记录日志
log_file			"syslog"
# 音频输出 根据安装的 包确定 自用 pipewire
audio_output {
	type		"pipewire"
	name		"PipeWire Sound Server"
}

```