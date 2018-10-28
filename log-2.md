---
title: log-2
date: 2018-09-01 17:45:48
tags:
---

ArchLinux Grub引导的坑

安装Grub。首先在新系统里面装：

```bash
# pacman -S grub
```

安装Grub要先chroot。。。

```bash
# arch-chroot /mnt
```

GPT的小修改
分区的时候，分一个+1m的区，设置为BIOS boot(代码是4)。

安装Grub

```bash
# grub-install --target=i386-pc /dev/sda2
```

生成主配置文件。。。不然GPT会抽风

```bash
# grub-mkconfig -o /boot/grub/grub.cfg
```