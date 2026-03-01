# xorg

Xorg (commonly referred to as simply X) is the most popular display server among Linux users.

## startx

Linux 提供虚拟控制台的功能，一组终端设备共享PC电脑的屏幕、键盘和鼠标。通常一个 Linux 安装配置 8 个或者 12 个虚拟控制台。

虚拟控制台通过字符设备文件 `/dev/ttyN` 使用，其中 `N` 代表一个数字，从 1 开始。

使用字符界面登陆 Linux 系统，所使用的终端设备就是系统中的第一个虚拟控制台，即终端设备 `/dev/tty1`。

当已经在字符界面时（例如 `TTY3`），在字符界面之间切换（例如切换到 `TTY4`），可以直接按 `Alt + F4`（不需要按住 `Ctrl`）。

从字符界面切回图形界面，直接按下 `Ctrl + Alt + F7` 即可。KDE 的 `SDDM` 和 GNOME 的 `GDM` 是显示管理器<sub>display manager</sub>，默认使用第一个未使用的虚拟控制台，通常是 `/dev/tty7`。

从图形界面切回字符界面，使用 `Ctrl+Alt+F<N>` 在不同的虚拟控制台之间切换，其中 `N` 是虚拟控制台 `/dev/ttyN` 所对应的数字。如你正在使用图形界面（如 GNOME 或 KDE），按下 `Ctrl + Alt + F3`（或 `F2`-`F6` 任意一个未被占用的），屏幕会立即切换到黑色的字符登录界面。

默认情况，X 视窗系统从 `session:0` 启动。要在 Linux 系统上运行多个 X 视窗会话必须告诉 `startX` 启动 `session:1`。

命令 `startx -- :1`, Linux 将在下一个未使用的虚拟控制台上启动 X 服务器。

在一个 Xsession 中启动 `gkrellm`，并让其在另一个 Xsession 中显示 `DISPLAY=:1 gkrellm &`。
