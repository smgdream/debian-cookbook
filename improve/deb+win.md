# Debian + Windows
部分人有使用Debian和windows双系统的需要，在此说明一下安装双系统要注意的点以及一些建议。  


## 关于系统的安装顺序
在比较老旧的资料中可能建议先装Windows再装Linux否则会造成引导出错。经过笔者的测试如果使用Dism++安装Windows 10，则能正常处理双系统引导的问题（但没有使用Windows原生安装程序测试过），系统的安装顺序不再重要。

## Debian读写NTFS文件系统
Debian Trixie的ntfs驱动规更换成了ntfs3。其虽然提供了更好的NTFS文件系统读写性能，但在实际使用上可能存在一些小问题（见：[当前Debian的部分Bugs](../start/bugs.md)）。  

## Debian与Windows共享数据分区
部分用户有Debian和Windows共享数据分区的需求，而数据共享分区建议使用UFS或exFAT文件系统而非NTFS（因为ntfs3驱动对NTFS的支持仍不完善）。至于ext4和Btrfs文件系统虽然有大佬开发的适用于Windows的驱动（[Ext4Fsd](https://github.com/bobranten/Ext4Fsd)和[WinBtrfs](https://github.com/maharmstone/btrfs)），但存在不少bug，故不建议使用。  

注1：UFS和exFAT对意外断电导致的数据损坏的修复能力较差，请尽量避免意外断电。  

注2:  如果使用NTFS作为数据共享分区建议关闭Windows中磁盘的“启用设备上的写缓存功能”以及将磁盘的删除策略更改为快速删除，并且关闭Windows的快速启动和休眠功能。如果不这样做，ntfs3可能无法成功挂载NTFS分区。

注3: 一个名为NTFS（旧称: NTFSPlus）的新NTFS文件系统驱动现已并入Linux (7.1+)之中，其相比ntfs3提供了更好的性能和更完善的NTFS支持，值得期待。<!-- NTFS驱动核心开发者Namjae Jeon维护的配套NTFS文件系统工具ntfsprogs-plus: https://github.com/ntfsprogs-plus/ntfsprogs-plus -->  

## Windows读Debian分区
长期以来Windows读Linux系统分区并没有一个很好的解决方案。如，DiskGenius只支持读少数几种分区格式，Ext2Fsd/Ext4Fsd仅支持Ext系列分区格式且存在不少bug，WinBtrfs只支持Btrfs格式且可能会破坏分区数据。这给Windows+Linux双系统用户造成了不便。

[FsRover](https://github.com/a1ive/FsRover)（原NkArc）的出现使得处境得到了极大的改善。FsRover是一个基于GRUB 2代码的开源多文件系统只读文件浏览器。他可以读取并提取Ext系列、XFS、ZFS、Btrfs等文件系统中的文件，还可以将支持的文件系统的分区挂载为只读虚拟磁盘。我们可以通过FsRover来方便地读取Linux分区中的文件。

注：FsRover需要以管理员身份运行。如果要挂载虚拟磁盘还要在软件内安装Dokan虚拟磁盘驱动组件，Windows 7则建议自行安装[WinFsp](https://winfsp.dev/)虚拟磁盘驱动组件。

## Debian和Windows共存的时间问题

如果你使用同时使用Debian和Windows时你将发现用完Debian切换到Windows后时间少了8小时（对于UTC+8时区的用户），反之亦然。这是因为Debian和windows对硬件时钟的时间的处理不一样。Debian将硬件时钟的时间当作UTC时间，然后在软件层面将时间转换为当前时区的时间。而windows直接将硬件时间当作当前时区的时间。由此导致了切换系统后时间对不上的问题。  

此问题的解决方法为将其中一个系统的时间模式设置为与另一个系统一样的时间模式。下面将说明如何将Windows的时间模式设置为与Debian一样的将硬件时钟时间作为UTC时间的模式。  

以管理员身份执行命令提示符并执行以下命令。  
```bat
reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
```
说明：通过命令添加注册表项，让Windows把硬件时钟的时间当作UTC时间。  

## 其他建议
- 建议在UEFI/BIOS设置中把Debian设置为第一启动项。  

## 参考资料
\[1\] [[PATCH 00/11] ntfsplus: ntfs filesystem remake](https://lore.kernel.org/lkml/20251020020749.5522-1-linkinjeon@kernel.org/)  
\[2\] [kernel/git/linkinjeon/ntfs.git - NTFS+ filesystem](https://git.kernel.org/pub/scm/linux/kernel/git/linkinjeon/ntfs.git/log/?h=ntfs-next)  
\[3\] [ntfsplus « fs - kernel/git/linkinjeon/ntfs.git - NTFS+ filesystem](https://git.kernel.org/pub/scm/linux/kernel/git/linkinjeon/ntfs.git/tree/fs/ntfsplus?h=ntfs-next)  
\[4\] [The Linux NTFS+ filesystem driver](https://git.kernel.org/pub/scm/linux/kernel/git/linkinjeon/ntfs.git/tree/Documentation/filesystems/ntfsplus.rst?h=ntfs-next)  
\[5\] [ntfsprogs-plus](https://github.com/ntfsprogs-plus/ntfsprogs-plus)  
\[6\] [AUR (en) - ntfsplus-dkms-git](https://aur.archlinux.org/packages/ntfsplus-dkms-git)  
\[7\] [Merge tag 'ntfs-for-7.1-rc1-v2' of git://git.kernel.org/pub/scm/linux/kernel/git/linkinjeon/ntfs](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=cdd4dc3aebeab43a72ce0bc2b5bab6f0a80b97a5)  
\[8\] [Re: [GIT PULL] ntfs updates for 7.1-rc1(v2)
\- by Linus Torvalds @ 2026-04-18  0:01 UTC [4%]](https://lore.kernel.org/lkml/CAHk-=whkM5Ju-Meiy0G-a-+LwmbwA-jaRR_d-Ka5X6MnG95xeg@mail.gmail.com/)  
\[9\] [[原创] 开源的文件系统浏览器[20260817] (支持UD, APFS, ReFS等)](https://bbs.wuyou.net/forum.php?mod=viewthread&tid=438290)  
\[10\] [FsRover](https://github.com/a1ive/FsRover)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.6 | Date: 2026-09-03
