+++
title = 'nmap'
date = '2024-05-25'
author = 'aiclr'
categories = ['software']
tags = ['nmap','archlinux']
Summary='Nmap (“Network Mapper”) is an open source tool for network exploration and security auditing. It was designed to rapidly scan large networks, although it works fine against single hosts.'
+++

[arch wiki](https://wiki.archlinux.org/title/Nmap)

### install

```sh
pacman -S nmap
```

### 使用方法

nmap [扫描类型](#扫描类型) [通用选项](#通用选项) [扫描目标](#扫描目标)

使用 TCP SYN 发现活跃主机
```sh
nmap -PS 192.168.1.0/24
```
使用 TCP 方式扫描端口
```sh
nmap -sS -sV -n -p1-65535 -oX tcp.xml 192.168.1.0
```
使用 UDP 方式扫描端口
```sh
nmap -sS -sU -n -p1-65535 -oX udp.xml 192.168.1.0
```
判断操作系统
```sh
nmap -O 192.168.1.0/24
```
判断服务版本
```sh
nmap -sV 192.168.1.0/24
```

扫描结果端口状态说明
- ***open*** 目标端口开启
- ***closed*** 目标端口关闭
- ***filtered*** 通常被防火墙拦截，无法判断目标端口开启与否
- ***unfiltered*** 目标端口可以访问，但无法判断开启与否
- ***open|filtered*** 无法确定端口是开启还是filtered
- ***closed|filtered*** 无法确定端口是关闭还是filtered

端口分类
- 公认端口 ***well-known port: 0-1024*** 最常用端口，通常于协议绑定
- 注册端口 ***registered port: 1025-49151*** 最常用端口，通常于协议绑定
- 动态或私有端口 ***dynamic/private port: 49152-65535*** 最常用端口，通常于协议绑定
- 特例: 端口还与协议相关。比如 ***UDP*** 端口 ***53*** 通常用于 DNS 查询、***TCP*** 端口 **53** 通常用于 DNS 记录迁移

### 扫描类型

***-sT*** TCP conect()扫描，最基本的TCP扫描方式。 \
connect() 是一种系统调用，由操作系统提供，用来打开一个连接 \
如果目标端口有程序监听，connect() 就会成功返回，否则这个端口是不可达的。 \
无需 root 权限，任何 unix 用户都可以自由使用这个系统调用。 \
这种扫描很容易被检测到，在目标主机的日志中会记录大批的连接请求以及错误信息

***-sS*** TCP 同步扫描 TCP SYN，因为不必全部打开一个 TCP 连接，被称为半开扫描 half-open。 \
发出一个TCP 同步包 SYN，然后等待回应。 \
如果返回 SYN|ACK(响应) 包，表示目标端口正在监听， \
&nbsp;&nbsp;&nbsp;&nbsp;源主机马上发出一个RST(复位)数据包断开喝目标主机的连接，实际由操作系统内核自动完成。 \
如果返回 RST 数据包，表示目标端口没有监听程序。 \
好处：很少有系统能够把这记入系统日志。不过需要 root 权限 定制 SYN 数据包

***-sU***
UDP 扫描，发送0字节UDP包，快速扫描 Windows 的UDP端口。 \
使用此扫描方法，可以获取目标主机上提供哪些UDP(用户数据报协议 RFC768)服务 \
nmap 首先向目标主机的每个端口发出一个0字节的UDP报 \
收到端口不可达的ICMP消息，端口就是关闭的，否则就是打开的

***-sP*** ping 扫描，仅想获取哪些主机正在运行，通过向指定的网络内的每个IP地址发送ICMP echo 请求数据包即可。 \
注意,nmap 在任何情况下都会进行 ping 扫描，只有目标主机处于运行状态，才会进行后续的扫描 \
如果只想知道目标主机是否运行，不想进行其他扫描，才使用这个选项

***-sA*** ACK扫描，TCP ACK扫描，当防火墙开启时，查看防火墙有未过滤某端口。 \
通常用来穿过防火墙规则集。有助于确定一个防火墙是功能比较完善的或是一个简单的包过滤程序，只是阻塞进入的SYN包。 \
这种扫描是向特定端口发送 ACK 包(使用随机的应答/序列号) \
如果返回一个 RST包，这个端口就标记为 unfiltered 状态 \
如果什么都没有返回，或者返回一个不可达 ICMP消息，这个端口就归入 filtered 类。 \
注意 nmap 通常不输出 unfiltered 的端口，在输出中不显示所有被探测的端口 \
这种扫描方式不能找出处于打开状态的端口

***-sW*** 滑动窗口扫描，非常类似于 ACK 扫描，除了它有时可以检测到处于打开状态的端口。 \
因为滑动窗口的大小是不规则的，有些操作系统可以报告其大小

***-sR*** RPC扫描，和其他不同的端口扫描方法结合使用

***-b*** FTP 反弹攻击(FTP Bounce attack)外网用户通过 FTP渗透内网

### 通用选项

***-P0*** nmap 扫描前 不Ping 目标主机。有些防火墙不允许ICMP echo 请求穿过，这个选项可以对这些网络进行扫描

***-PT*** nmap 扫描前 使用 TCP ACK 包确定目标主机是否在运行 \
-PT 默认80端口。 扫描之前，使用 TCP ping 确定哪些主机正在运行。 \
nmap不是通过发送 ICMP echo 请求包，然后等待响应来实现这种功能 \
而是向目标网络或者单一主机发出 TCP ACK包然后等待回应 \
如果主机正常运行就会返回 RST包，只有在目标网络/主机阻塞了ping包，而仍旧允许扫描时，这个选项才有效 \
对于非root用户，使用 connect() 系统调用来实现这个功能 \
可以使用 -PT 来设定目标端口 ，默认80 这个端口通常不会被过滤

***-PS*** nmap使用 TCP SYN包进行扫描 \
对于 root 用户 这个选项让nmap 使用 SYN 包而不是 ACK包对目标主机进行扫描。 \
如果主机正在运行返回一个 ***RST包(或者一个 SYN/ACK包)***

***-PI*** nmap 进行ping扫描，设置这个选项，让nmap使用真正的 ***ping（ICMP echo请求）***来扫描目标主机是否正在运行。 \
这个选项让nmap发现正在运行的主机的同时，nmap也会对直接子网广播地址进行观察 \
直接子网广播地址一些外部可达的IP地址，把外部的包转换为一个内向的IP广播包，向一个计算机子网发送。\
这些IP广播包应该删除，因为会造成拒绝服务攻击 例如 smurf


***-PB*** 结合 ***-PT*** 和 ***-PI*** 功能，这是默认的 ping 扫描选项，使用 ***ACK（-PT）***和 ***ICMP（-PI）***两种扫描类型并行扫描 \
如果防火墙能够过滤其中一种包，之后使用这种方法，就能穿过防火墙

***-O*** nmap扫描TCP/IP指纹特征，确定目标主机系统类型

***-I*** 反向标志扫描，扫描监听端口的用户

***-f*** 分片发送 ***SYN FIN Xmas Null*** 扫描的数据包

***-v*** 冗余模式扫描，可以得到扫描详细信息

***-oN*** 扫描结果重定向到文件

***-resume*** 使被中断的扫描可以继续

***-iL*** 扫描目录文件列表

#### -p

指定端口或扫描端口列表及范围，默认扫秒 ***1-1024***端口和 ***/usr/share/nmap/nmap-services***文件中指定的端口
例：
- ***-p 23*** 只扫描目标主机的 ***23*** 端口
- ***-p 20-30,139,60000-*** 扫描 ***20~30***端口，***139***端口，所有大于***60000***的端口

### 扫描目标

- ***192.168.1.25***
- ***192.168.1.0/24*** 表示：***192.168.1*** 网段所有ip
- ***192.168.*.**** 表示 ***192.168.1.1~ 192.168.255.255*** 所有ip