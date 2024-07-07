+++
title = 'Linux'
date = '2024-07-06'
author = 'aiclr'
categories = ['os']
tags = ['linux']
Summary='linux 基础知识'
+++


## gunzip

解压
```shell
gunzip -c xxx.log.gz > ./xxx.log
```

压缩
```shell
gzip -c xxx.log > ./xxx.log.gz
```

## zip

解压
```shell
unzip xxx.zip -d ./unzip/
```

解压 windows 中文乱码
```shell
unzip -O CP936 xxx.zip -d ./unzip/
```

压缩
```shell
zip -r xxx.zip ./xxx
```

## jar

解压war包
```shell
jar -xvf **.war
```

解压jar包
```shell
jar -xvf flyos-dev-2.1.9.3.jar
```

压缩jar包
```shell
jar -cfM0 flyos-dev-2.1.9.3.jar BOOT-INF META-INF org
```
 
## tar

不解压列出tar内容
```shell
tar -tvf **.tar
```

将file文件夹添加到**.tar
```shell
tar -cvf **.tar ./file/
```

解压tar 到file
```shell
tar -xvf **.tar ./file
```

## Shell:即工作环境

**user**通过**shell**操作硬件,**windows**操作系统只是把**shell**封装成**可视化界面**进行与硬件交互

用户登陆

**~** 表示当前用户目录 **/home/用户名**

**/** 表示 **/root**<sub>根目录</sub>

**pwd** 查看当前所在目录

**#** 为**root**提示符号

**$** 为普通用户

<br/>
<hr/>
<br/>

命令

**command 【option】<sub>命令选项</sub> 【arguments】<sub>命令参数</sub>** 例如 **ls -l /** 查看 **/** 目录下的目录详细信息

**date** 时间

**cal** 日历

**bc** 计算器

**man cal** 和 **info cal** 查看 **命令参数** 命令的帮助文档

<br/>
<hr/>
<br/>

关机命令

**shutdown** 

**-h** 关机 **-r** 重启 **-f** 快速重启

**hh:mm** 执行时间 21:23 21点23分执行

**+m** m分钟后执行

**now** 即+0 立即执行

还可以加广播提示其他用户

```shell
shutdown -h +10 'the system will shutdown'
```

**centos** **archlinux** 可以执行 **poweroff** 关机

<br/>
<hr/>
<br/>

显示文件系统内容 \
Linux提供多用户多任务环境操作实现，文件可存取访问的身份owner，group，other，权限read，write，execute（可执行）
- read = r = 4
- write = w = 2
- execute = x =1

```sh
# 显示当前目录更多文件信息
ls -l
# 显示当前目录隐藏文件
ls -a
# 显示当前目录全部文件+文件信息
ls -al
```

`drwxr-xr-x. 2 root root 6 Jul 23 10:12 study`

|    文件权限    | 链接数 | 文件所有者 | 文件所属用户组 | 文件大小 |   文件最后修改时间   |  文件名  |
|:----------:|:---:|:-----:|:-------:|:----:|:------------:|:-----:|
| drwxr-xr-x |  2  | root  |  root   |  6   | Jul 23 10:12 | study |


 文件权限

|  d   |   rwx   |    r-x    |    r-x     |
|:----:|:-------:|:---------:|:----------:|
| 文件类型 | 文件所有者权限 | 文件所属用户组权限 | 其他人对此文件的权限 |

- 文件类型
    - d:目录
    - -:文件
        - 纯文本文件（ASCII）
        - 二进制文件（binary）
        - 数据格式文件（date）
    - i:连接文件，类似windows下的快捷方式
    - b:设备与设备文件，可存储接口设备与系统外设及存储相关/块设备，在/dev下
    - c:串行端口设备，字符设备文件
    - s:套接字，数据接口文件，网络上的数据连接 /var/run
    - p:管道（FIFO，PIPE） 解决多个程序访问一个文件时造成的错误问题
- 文件扩展名:只是为了让用户了解文件的用途。linux文件是没有所谓的扩展名的，一个文件是否执行与文件权限的 **【drwxr-xr-x】** 十个属性相关，有x属性则表示可以执行，但是可以执行与执行成功并不一致
    - 常用扩展名
        - `*.sh` 脚本或批处理文件 script 脚本是用shell写成的
        - `*.Z,*.tar,*.tar.gz,*.zip,*.tg`经打包的压缩文件
- 改变文件属性和权限
    - 改变文件所属用户组 `chgrp groupName file`
    - 改变文件所有者 `chown userName file`
    - 改变文件的权限 `chmod 777 file`
- 权限计算方法:777是rwxrwxrwx的累加，如下
    - owner = rwx = 4+2+1 = 7
    - group = rwx = 4+2+1 = 7
    - other = rwx = 4+2+1 = 7
```sh
# 显示/目录下的文件
ls -a -l /
# 列出目录下文件名及其目录类型（后跟*号表示可执行文件，@表示符号连接/表示目录名）
ls -F
# 按照最后修改文件时间列出文件名
ls -t
# 列出当前目录及其子目录的文件名
ls -R
```

<br/>
<hr/>
<br/>

### 操作目录和文件

```sh
# 在当前目录下创建目录 myfile
mkdir myfile

# 删除当前目录下的myfile目录（myfile目录下必须为空才能删除成功）
rmdir myfile

# 返回上级目录 . 表示当前目录 .. 表示上级目录
cd ..

# 复制当前目录下的XX.log文件到 ./ 目录下的one目录内 即当前目录下的one目录内
cp XX.log ./one

# 当前目录下将目录one复制到two内 -r表示可以复制目录及目录内的所有文件（递归）
cp -r one two

# 当前目录内复制xx.log并将新复制的文件改名为xx.log.bak
cp xx.log xx.log.bak

# 将当前目录下的xx.log文件移到当前目录下的one目录内
mv xx.log ./one

# 将当前目录下的xx.log移到当前目录下，并改名为xx.syslog（改名字）
mv xx.log xx.syslog

# 在当前目录下创建1.txt文件
touch 1.txt

# 查看文件类型
file 1.txt

# 删除当前目录下的1.txt文件，强制删除不是递归
rm -f 1.txt

# 循环删除目录，直到该目录下内容全部被删除，即递归
rm -r

# 删除当前目录下所有1开头的 **.txt** 文件 *号匹配任意字符和字符串
rm -f 1*.txt

rm -f 1?.txt

# 删除当前目录下以11开头1，2，3结尾的 **.txt** 文件[abc]表示a,b,c任一字符,[a-x]a到x的任一字符,[1-9]1到9任一数字
rm -f 11[1-3].txt

# 删除当前目录下第二个字符不是1的.txt文件 与上一条含义相反
rm -f 1[!1]*.txt

# 搜索xx.log文件；此操作需要数据库支持，默认7天更新一次，也可以手动更新执行updatedb需耗时
locate xx.log

# print working directory<sub>打印当前所在目录</sub>
pwd
```

<br/>
<hr/>
<br/>

用户：只有 **root** 可以设置其他用户密码，其他用户只能修改自己的密码

**useradd caddy** 增加 **caddy** 用户<sub>root权限</sub>

**passwd caddy** 修改 **caddy** 密码<sub>root权限</sub>

**passwd** 修改自己密码，不用指定用户名<sub>caddy权限</sub>

### 显示文件内容

```sh
# 适合显示内容少的，如果太多行，只会显示后面的部分（看不全）
cat xx.log
# 解决 **cat** 显示不全的问题 按 **space** 翻页  **q** 键退出（不能倒退查看已经看过的内容）
more xx.log
# 解决 **more** 不能上下翻页的问题 **PageUp PageDown** 翻页 **q** 退出
less xx.log
# 显示文件前几行（默认10行）
head xx.log
# 显示文件最后几行（默认10行）
tail xx.log
# 显示前/后10行
head/tail -10 xx.log
# 从最后一行开始显示文件内容
tac xx.log
# 显示并输出行号
nl xx.log
# 以二进制读取文件
od xx.log
```

## 进程监控

### top & htop

```shell
[caddy@blackbox ~]$ top

pacman -S htop
[caddy@blackbox ~]$ htop
交互式的实时linux进程监控工具
```

## 监控磁盘I/O

### iotop

```shell
pacman -S iotop
[caddy@blackbox ~]$ iotop
监控并显示实时磁盘I/O、进程的统计功能。
在查找具体进程和大量使用磁盘读写进程的时候，这个工具就非常有用
只有在kernelv2.6.20及以后的版本中才有。python版本需要 python2.7及以上版本
```

## 输入/输出统计

### iostat

```shell
pacman -S sysstat
[caddy@blackbox ~]$ man iostat
收集显示系统存储设备输入和输出状态统计的简单工具
踪存储设备的性能问题，其中存储设备包括设备、本地磁盘，以及诸如使用NFS等的远端磁盘
[caddy@blackbox ~]$ iostat 
Linux 5.11.10-arch1-1 (blackbox) 	03/28/2021 	_x86_64_	(12 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.48    0.00    0.25    0.03    0.00   99.25

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
nvme0n1          10.74       320.47        39.38         0.00     965612     118658          0
```

- avg-cpu
    - %user: 在用户级别运行所使用的CPU的百分比.
    - %nice:优先进程消耗的CPU时间，占所有CPU的百分比.
    - %system: 在系统级别(kernel)运行所使用CPU的百分比.
    - %iowait: CPU等待硬件I/O时,所占用CPU百分比.
    - %steal: 管理程序维护另一个虚拟处理器时，虚拟CPU的无意识等待时间百分比.
    - %idle: CPU空闲时间的百分比.
- Device
    - tps: 每秒钟发送到的I/O请求数.
    - KB_read /s: 每秒读取的block数.
    - KB_wrtn/s: 每秒写入的block数.
    - KB_read: 启动到现在 读入的block总数.
    - KB_wrtn: 启动到现在写入的block总数.

## swap虚拟内存统计

### vmstat 采样时间间隔数 采样次数

```shell
vmstat 2 1
vmstat 2 2
vmstat 2
[caddy@blackbox ~]$ man vmstat
[caddy@blackbox ~]$ vmstat 2 2
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 30582452  75880 1105616    0    0    36     4  143  219  1  0 99  0  0
 0  0      0 30582576  75880 1105540    0    0     0     0  333  364  0  0 100  0  0
```

- r ：表示运行队列，如果运行队列过大，表示你的CPU很繁忙，一般会造成CPU使用率很高
- b ：表示阻塞的进程数swpd ：虚拟内存已使用的大小，如果大于0，表示你的机器物理内存不足了，如果不是程序内存泄露的原因，那么你该升级内存了或者把耗内存的任务迁移到其他机器
- free ：空闲的物理内存的大小
- buff ： 系统占用的缓存大小
- cache ：直接用来记忆我们打开的文件,给文件做缓冲
- si ：每秒从磁盘读入虚拟内存的大小，如果这个值大于0，表示物理内存不够用或者内存泄露了
- us ：用户CPU时间
- sy ：系统CPU时间
- so ： 每秒虚拟内存写入磁盘的大小，如果这个值大于0，同上。
- sy ： 系统CPU时间，如果太高，表示系统调用时间长，例如是IO操作频繁。
- id ： 空闲 CPU时间，一般来说，id + us + sy = 100
- wt ： 等待IO CPU时间。

## 列出打开的文件

lsof 显示所有打开的文件和进程。打开的文件包括磁盘文件、网络套接字、管道、设备和进程。主要情形之一就是在无法挂载磁盘和显示正在使用或者打开某个文件的错误信息的时候。使用这条命令，你可以很容易地看到正在使用哪个文件

## 网络状态统计

**Arch Linux has deprecated net-tools in favor of iproute2**

| Deprecated command | Replacement commands |
| :----------------- | :------------------- |
| arp                | ip neighbor          |
| ifconfig           | ip address, ip link  |
| netstat            | ss                   |
| route              | ip route             |

### netstat replace with ss 

监控网络性能，定位并解决网络相关问题
- [arch wiki](https://wiki.archlinux.org/title/Network_configuration#net-tools)
- [ss man page](https://man.archlinux.org/man/ss.8)

```shell
# Display man page
man ss
# Display all TCP Sockets with service names
ss -at
# Display all TCP Sockets with port numbers
ss -atn
# Display all UDP Sockets
ss -au
```


## 实时局域网IP监控

### iptraf

```shell
pacman -S iptraf-ng
[caddy@blackbox ~]$ sudo iptraf-ng
实时网络（局域网）监控应用
网络的IP流量监控，包括TCP标记、ICMP详细信息、TCP/UDP流量分离、TCP连接包和字节数。
同时还采集有关接口状态的常见信息和详细信息：
TCP、UDP、IP、ICMP、非IP，IP校验和错误，接口活动等
```