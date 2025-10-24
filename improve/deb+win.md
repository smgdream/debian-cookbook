# Debian + Windows
部分人有使用Debian和windows双系统的需要，在此说明一下安装双系统要注意的点。  

<!--
**关于系统的安装顺序**  
在比较老旧的资料中可能建议先装windows在装Linux否则会造成引导出错，但现在的引导程序已经能正确的处理双系统引导的问题了，系统的安装顺序不再重要。
-->  

## Debian读写NTFS文件系统
Debian Trixie的ntfs驱动规更换成了ntfs3，在实际使用上可能存在一些小问题（见：[当前Debian的部分Bugs](../start/bugs.md)）。  

## Debian与Windows共享数据分区
部分用户有Debian和Windows共享数据分区的需求，而数据共享分区建议使用UFS或exFAT文件系统而非NTFS（因为ntfs3驱动对NTFS的支持仍不完善）。至于ext4和Btrfs文件系统虽然有大佬开发的适用于Windows的驱动（[Ext4Fsd](https://github.com/bobranten/Ext4Fsd)和[WinBtrfs](https://github.com/maharmstone/btrfs)），但存在不少bug，故不建议使用。  

注1：UFS和exFAT对意外断电导致的数据损坏的修复能力较差，请尽量避免意外断电。  

注2:  如果使用NTFS作为数据共享分区建议关闭Windows中磁盘的“启用设备上的写缓存功能”以及将磁盘的删除策略更改为快速删除，并且关闭Windows的快速启动和休眠功能。如果不这样做，ntfs3可能无法成功挂载NTFS分区。

注3: 根据最新消息有一个称为NTFSPlus的新NTFS驱动正在开发中，据说其相比ntfs3提供了更好的性能和更完善的NTFS支持。可以期待一下。  

## Debian和Windows共存的时间问题

如果你使用同时使用Debian和Windows时你将发现用完Debian切换到Windows后时间少了8小时（对于UTC+8时区的用户），反之亦然。这是因为Debian和windows对硬件时钟的时间的处理不一样。Debian将硬件时钟的时间当作UTC时间，然后在软件层面将时间转换为当前时区的时间。而windows直接将硬件时间当作当前时区的时间。由此导致了切换系统后时间对不上的问题。  

此问题的解决方法为将其中一个系统的时间模式设置为与另一个系统一样的时间模式。下面将说明如何将Windows的时间模式设置为与Debian一样的将硬件时钟当UTC时间的模式。  

以管理员身份执行命令提示符并执行以下命令。  
```bat
reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
```
说明：通过命令添加注册表项，让Windows把硬件时钟的时间当作UTC时间。  

**启动建议**  
建议在UEFI设置中把Debian设置为第一启动项。  

## 参考资料
\[1\] [[PATCH 00/11] ntfsplus: ntfs filesystem remake](https://lore.kernel.org/lkml/20251020020749.5522-1-linkinjeon@kernel.org/)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.4 | Date: 2025-10-24
