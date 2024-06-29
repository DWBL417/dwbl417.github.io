---
layout: post
title: Ubuntu之Windows10 + Ubuntu 双系统引导修复
category: project
description: 06-29-2024 | windows and ubuntu boot repair
---


因重装 win10，导致系统启动，直接进入 win10 系统，ubuntu 系统菜单丢失（通过分区工具确认分区及数据都在）

## 检查BIOS启动顺序

开机进入BIOS，Ubuntu启动顺序调整到首位，如依然开机无法进入ubuntu再进行修复

## 一、具体修复过程
1. 第一步：制作 Ubuntu U盘安装盘

利用 YUMI 工具 ，制作 Ubuntu U盘安装盘，制作过程参考：: https://www.pendrivelinux.com/yumi-multiboot-usb-creator/

2. 第二步：U盘启动预览版 Ubuntu 系统

还是需要进入 Ubuntu 界面，但是并不需要安装（如果直接安装的话，以前在 Ubuntu 里面的文件可全部都没有了，所以万不得已，千万别这样做）。

3. 第三步：进入预览版 Ubuntu 系统

#打开终端，终端快捷键是 Ctrl+Alt+T，输入命令，添加 boot-repair 所在的源：(确保可以联网) 

	sudo add-apt-repository ppa:yannubuntu/boot-repair && sudo apt-get update

4. 第四步：执行修复命令

#安装 boot-repair 并且开启 boot-repair：

	sudo apt-get install -y boot-repair && boot-repair

点击 Recommended repair，过几分钟重启就行了。

## 二、常见问题

1.按照以上操作，修复未成功！

如果上面已经执行成功了，可以跳过此部，否则，我们可以自己输入命令进行修复：

sudo recommended repair

成功后，就会弹出我们的盘的各种信息以及引导的信息。

2. 误点击了第二项 Create a BootInfo summary

那你的开机启动界面将会出来一大堆你以前没见过的东西。 那样的话，你可以输入名令：

cd /boot/grub

接着输入:

sudo gedit grub.cfg

打开 grub.cfg 文件后，通过搜索找到 windows，然后把下面这些删去就和原来一样了。


\### BEGIN /etc/grub.d/25_custom ###

menuentry "efi/EFI/Boot/bootx64.efi" {

search --fs-uuid --no-floppy --set=root d000ed6a-5303-40aa-a517-af50e807c0e9

chainloader (${root})/efi/EFI/Boot/bootx64.efi

}

menuentry "efi/EFI/ubuntu/MokManager.efi" {

search --fs-uuid --no-floppy --set=root d000ed6a-5303-40aa-a517-af50e807c0e9

chainloader (${root})/efi/EFI/ubuntu/MokManager.efi

}

menuentry "Windows UEFI recovery bootmgfw.efi" {

search --fs-uuid --no-floppy --set=root A603-846C

chainloader (${root})/EFI/Microsoft/Boot/bootmgfw.efi

}

menuentry "Windows Boot UEFI recovery" {

search --fs-uuid --no-floppy --set=root A603-846C

chainloader (${root})/EFI/Boot/bkpbootx64.efi

}

menuentry "EFI/ubuntu/MokManager.efi sda2" {

search --fs-uuid --no-floppy --set=root A603-846C

chainloader (${root})/EFI/ubuntu/MokManager.efi

}

menuentry "Windows UEFI recovery LrsBootmgr.efi" {

search --fs-uuid --no-floppy --set=root 7607-5674

chainloader (${root})/efi/Microsoft/Boot/LrsBootmgr.efi

}

menuentry "Windows Boot UEFI recovery bkpbootx64.efi" {

search --fs-uuid --no-floppy --set=root 7607-5674

chainloader (${root})/efi/Boot/bkpbootx64.efi

}
\### END /etc/grub.d/25_custom ###
