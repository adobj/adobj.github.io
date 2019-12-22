---
title: 修复Windows引导及挂载硬盘
date: 2019-12-22 12:21:49
tags:
---


Windows Diskpart命令详解


通过win安装进入系统修复-高级-进入命令行模式。

list disk - 显示磁盘列表。例如，LIST DISK。
list partition - 显示所选磁盘上的分区列表。例如，LIST PARTITION。
list volume - 显示卷列表。例如，LIST VOLUME。
list vdisk - 显示虚拟磁盘列表。


diskpart -显示Diskpart 版本及当前计算机名称
select disk n -焦点设置给具有指定Windows NT磁盘号n的磁盘，n可以有list disk命令查看。
如果未指定磁盘号，该命令将显示当前处于焦点的磁盘。
select partition n -将焦点设置给指定分区。如果未指定分区，则显示当前处于焦点的分区。
select volume x -将焦点设置给指定卷。如果未指定卷，该命令将显示当前处于焦点的卷。
select vdisk file=x:\xxx.vhd -焦点设置给指定的虚拟磁盘文件。


list disk

列出所有挂载磁盘


list par

列出当前磁盘分区


sel disk n

选择磁盘 n是编号


sel par n

同理