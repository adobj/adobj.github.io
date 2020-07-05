---
title: 修复Windows引导及挂载硬盘
tags:
  - 系统
abbrlink: 12a6
categories: uncategorized
date: 2019-12-22 12:21:49
---

本教材针对UEFI+GPT启动windows的efi分区损坏的情况下,使用原生windows启动盘进行修复。当然,也可以使用各种PE修复,此方式不在本次教程讨论范围。注意:不是丢失引导,这种情况可以使用EasyUEFI重建引导即可。
主要使用命令:diskpart、bcdboot
需要工具：装有win10镜像的U盘([MSDN](https://msdn.itellyou.cn)镜像+[Rufus](http://rufus.ie)启动盘创建工具+8G以上U盘)

<!--more-->	

## 通过启动盘进入命令提示符

1) 插入U盘,开机按快捷键(不同机型对应的快捷键自行百度)进入bios，选择U盘启动，有BIOS安全启动的先暂时关闭。顺利进入windows安装界面。
![安装界面](/images/winuefi/winstart1.jpg)	
2)我们直接无脑下一步下一步，直到'现在安装'界面。
![安装界面](/images/winuefi/winstart2.jpg)
点击左下角的'修复计算机'按钮。在出现的界面依次选择疑难解答->高级工具->命令提示符(不同版本可能有细微差别,总之目的是进入命令行)，这个命令行是以管理员身份运行，权限较高。
![安装界面](/images/winuefi/winstart3.jpg)

## 使用diskpart命令找到efi分区并分配盘符

| 命令 | 描述 |
|  ----  | ----  |
| list disk | 显示挂载磁盘列表 |
| list partition (或 list par) | 显示所选磁盘上的分区列表 |
| list volume | 显示卷列表 |
| list vdisk | 显示虚拟磁盘列表 |
| select disk n (或 sel disk n) | 焦点设置给具有指定Windows NT磁盘号n的磁盘，n可以有list disk命令查看。如果未指定磁盘号，该命令将显示当前处于焦点的磁盘。 |
| select partition n (或 sel par n) | 将焦点设置给指定分区。如果未指定分区，则显示当前处于焦点的分区。 |
| select volume n (或 sel volume n) | 将焦点设置给指定卷。如果未指定卷，该命令将显示当前处于焦点的卷。 |
| select vdisk n (或 sel vdisk n) | 焦点设置给指定的虚拟磁盘文件。 |

1)在命令行输入diskpart进入工具。
2)list disk列出所有挂载磁盘，sel disk n选择windows安装的磁盘
3)list par列出该磁盘上所有分区，sel par n选择efi分区，一般efi分区默认100m大小，如手动设置自己根据分区大小自行选择。
![安装界面](/images/winuefi/winstart4.jpg)
4)如果上一步，无法确认efi分区，可以使用list volume列出所选磁盘下所有的卷列表，然后sel volume n选择efi所在卷，efi分区FS为FAT。如果上一步已经设置选择分区，此步骤可跳过。
![安装界面](/images/winuefi/winstart5.jpg)
5)用assign命令给刚才选择的分区分配一个盘符，以便修复它，我这里给他一个o。
6)退出diskpart工具。
![安装界面](/images/winuefi/winstart6.jpg)

## 使用bcdboot命令修复分区
引导的本质就是告诉BIOS，要启动的系统在哪。修复它，就是要把启动需要的引导文件写到efi分区。前面已经知道efi分区的位置了，还给他分配了盘符o，因此要修复它还需要知道windows的系统在哪里，通常是c盘，也就是c:\windows，现在我们就要通过bcdboot命令来修复引导分区。
![安装界面](/images/winuefi/winstart7.jpg)
这里要注意的是系统盘可能在此命令行模式下分配的盘符不是C，所以需要自行确认系统盘的盘符。(怎么确定呢？dir进去一个一个看喽)
![安装界面](/images/winuefi/winstart8.jpg)
好，假如我们最终发现系统在D盘下，那完整的命令就是：
```
bcdboot d:\windows /s o: /f uefi /l zh-cn
```
执行之后，提示'已成功创建启动文件'就表示引导添加成功了，exit退出命令行，随后点击退出并继续。
![安装界面](/images/winuefi/winstart9.jpg)
至此，修复完成。重启后即可进入电脑。