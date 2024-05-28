+++
title = 'GRUB'
date = '2024-05-25'
author = 'aiclr'
categories = ['os']
tags = ['grub','archlinux']
Summary='GRUB (GRand Unified Bootloader) is a boot loader. '
+++

[win10 + archlinux 多系统手动添加引导参考](https://wiki.archlinux.org/title/GRUB#Windows_8/10_not_found)

首先查看分区
```sh
lsblk --fs
```
挂载win10 boot所在分区
```sh
mount /dev/sdd2 /mnt
```
查看 ***$hints_string***命令
```sh
grub-probe --target=hints_string /mnt/EFI/Microsoft/Boot/bootmgfw.efi
```
查看 ***$fs_uuid***命令
```sh
grub-probe --target=fs_uuid /mnt/EFI/Microsoft/Boot/bootmgfw.efi`(`lsblk --fs` 也能看到)
```
编辑 ***/boot/grub/grub.cfg***
```sh
  if [ "${grub_platform}" == "efi" ]; then
  	menuentry "Microsoft Windows Vista/7/8/8.1 UEFI/GPT" {
  		insmod part_gpt
  		insmod fat
  		insmod chain
  		search --no-floppy --fs-uuid --set=root $hints_string $fs_uuid
  		chainloader /EFI/Microsoft/Boot/bootmgfw.efi
  	}
  fi
```
example:
```sh
### BEGIN /etc/grub.d/30_os-prober ###
if [ "${grub_platform}" == "efi" ]; then
	menuentry "Microsoft Windows Vista/7/8/8.1/10 UEFI/GPT" {
		insmod part_gpt
		insmod fat
		insmod chain
		search --no-floppy --fs-uuid --set=root --hint-bios=hd3,gpt2 --hint-efi=hd3,g pt2 --hint-baremetal=ahci3,gpt2 C018-7605
		chainloader /EFI/Microsoft/Boot/bootmgfw.efi
	}
fi
### END /etc/grub.d/30_os-prober ###
```

### problem

小米游戏本开机变慢发现是 ***systemd-journal-flush.service*** 耗时30秒+

查看日志文件大小40M
```sh
journalctl -b -u systemd-journald
```
查看启动耗时
```sh
journalctl -b -u systemd-journal-flush.service
```