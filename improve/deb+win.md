# Debian + Windows
部分人有使用Debian和windows双系统的需要，在此说明一下安装双系统要注意的点。  

**关于系统的安装顺序**  
在比较老旧的资料中可能建议先装windows在装Linux否则会造成引导出错，但现在的引导程序已经能正确的处理双系统引导的问题了，系统的安装顺序不在重要。  

**Debian读写NTFS文件系统**  
Debian Trixie的ntfs驱动规更换成了ntfs3，在实际使用上可能存在一些小问题（见：[当前Debian的部分Bugs](../start/bugs.md)）。  

**Debian与Windows共享数据分区**  
部分用户有Debian和Windows共享数据分区的需求，而数据共享分区建议使用UFS或exFAT文件系统而非NTFS（因为ntfs3驱动对NTFS的支持仍不完善）。至于ext4和Btrfs文件系统虽然有大佬开发的适用于Windows的驱动（[Ext4Fsd](https://github.com/bobranten/Ext4Fsd)和[WinBtrfs](https://github.com/maharmstone/btrfs)），但存在不少bug，故不建议使用。  

**启动建议**  
建议在UEFI设置中把Debian设置为第一启动项。  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.4.5 | Date: 2025-08-09
