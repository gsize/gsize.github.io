---
layout: post
title: 安装Archlinux流程
description: 记录安装Archlinux系统过程
category: blog
---

# 安装Archlinux系统

在thinkpad 201i 上安装Archlinux，至于为什么要安装这个系统，而不选用Windows，或是其他Linux版本，就不仔细说明。
Archlinux系统具体有两大特点：1、滚动升级，无所谓的大版本升级不兼容问题；
2、系统深度定制，根据个人喜好安装软件，以达到对系统的高度灵活配置，从而实现系统整体高度精简。

安装系统分三个步骤：
1、系统安装前期准备。如U盘引导盘的制作，
2、硬盘格式化,fdisk；
3、挂载分区,mount;
4、配置archlinux升级源，国内推荐mirrors.163.com、mirrors.aliyun.com等
5、连接网络，用wifi-menu命令连接wifi;如果是有线网络，则用ip连接。
6、安装archlinux基本系统，pacstrap /mnt base base-devel
7、生成fstab文件，genfstab -p /mnt >> /mnt/etc/fstab
8、arch-chroot /mnt切换根目录
9、设置系统时间：hwclock --systohc -utc 
10、设置语言：vi /etc/locale.gen ；运行locale-gen；
11、设置root密码；创建普通用户：useradd -m -g users -G wheel -s /bin/bash gsize 
12、安装系统启动器，一般用grub：pacman -S grub-bios ; grub-install --target=i386-pc /dev/sda ; grub-mkconfig -o /boot/grub/grub.cfg 
13、创建初始ramdisk环境： mkinitcpio -p linux 注意：如果分区有用到lvm，需先在/etc/mkinitcpio.conf文件中的HOOKS="base udev autodetect modconf block lvm2 filesystems keyboard fsck"加入lvm2参数
14、安装其他软件，也可以重启系统再安装。
15、exit推出chroot；卸载分区，并重启系统。


[Gsize]:    http://gsize.github.io  "Gsize"
