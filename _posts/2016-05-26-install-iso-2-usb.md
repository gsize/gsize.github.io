---
layout: post
title: 用U盘制作Archlinu系统安装引导 
description: 制作U盘系统引导，适用于linux系统安装。
category:  blog 
---

本文使用[extlinux][2]作为U盘引导程序，在windows系统下可以用[syslinux][1]制作引导程序。

Archlinux系统安装盘可以到[Archlinux的][1]下载最新的iso文件，但刻录光盘可不是随时都有的。如今的4-8G u盘已是主流。
实验室有一台2011年的dell t410的服务器（裸机），配置好linux，装入Geant4和ROOT软件，实现核物理蒙卡模拟功能。

由于我的工作机是fedora16，手头又有8g的u盘，省得刻录。接下来开始工作了！！

## 制作用于引导安装Archlinux系统的U盘 ##

在开始之前，先把u盘格式化位ext2或ext3格式，这样就能放入大于4G的iso镜像文件。（关于格式化，可以看看[鸟哥的Linux私房菜][4]。）

1、下载系统启动引导工具软件：syslinux，，syslinux-4.04下载在http://www.linuxidc.com/Linux/2011-05/36594.htm版本

到http://www.scientificlinux.org/下载linux镜像文件。

2、终端下，用wget  -c下载好syslinux-4.04后，用 tar -jxvf 命令把syslinux-4.04.tar.bz2文件解压缩出来；

3、在终端里进入解压的syslinux-4.04目录下；

4、执行sudo ./extlinux/extlinux -i   /nmt/usb(你的u盘挂载点)，会在u盘里产生一个叫 ldlinux.sys的文件；

5、exlinux 生成的引导文件只是保存在U盘中的普通文件，需要改变MBR，来指向它。

所以要用 syslinux 包中附带的 mbr 覆盖U盘原来的mbr。并需要用 fdisk 将要启动的分区的 boot flag 设置为 on（fdisk的用法看《鸟哥的私房菜》去，linux.vbird.org）。

　　＃sudo cat ./mbr/mbr.bin > /dev/sdb（不过这样做没搞定，改用sudo dd if=./mbr/mbr.bin of=/dev/你的u盘识别符，一般都是sdb，注意了，不是sdb1之类的。）

6. 复制com32文件夹下的 menu.c32,vesamenu.c32文件到U盘根目录下面；

7. 提取SL-61-x86_64-2011-11-09-Install-DVD.iso（我下的是64位的，32位的是i686.iso） 里面 isolinux\下isolinux.cfg文件到u盘根目录下，并改名为syslinux.cfg

8. 提取 SL-61-x86_64-2011-11-09-Install-DVD.iso里面 isolinux\下vmlinuz,initrd.img 到U盘根目录下；

9. 复制SL-61-x86_64-2011-11-09-Install-DVD.iso 里面 images目录到U盘根目录下；

10.DVD版本的启动U盘制作完成。目录结构如下：

├SL-61-x86_64-2011-11-09-Install-DVD.iso

├ ldlinux.sys

├ vmlinuz

├ initrd.img

├ vesamenu.c32

├ menu.c32

├ syslinux.cfg 

└ images/



现在的这个u盘就可以装linux系统了，不过要记得改bios，使得机子usb优先识别。

使用的u盘种类也是有讲究的，不能是那些具有特殊功能的u盘（例如带有杀毒的，或厂商特别设定的功能等等）。所以建议用普通的u盘。

windows下的系统，可以参考博客 http://wang020612.blog.163.com/blog/static/5982142920116161222949/



[1]:  http://www.syslinux.org/wiki/index.php?title=SYSLINUX  "syslinux"
[2]:  http://www.syslinux.org/wiki/index.php?title=EXTLINUX  "extlinux"
[3]:  https://www.archlinux.org  "Archlinux官网"
[4]:  http://mirrors.aliyun.com/archlinux/iso/latest/ "Archlinux iso"
[5]:  http://linux.vbird.org/linux_basic/0230filesystem.php#disk 
