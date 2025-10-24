# Debian Trixie 的新特性

## 架构
Debian Trixie移除了i386架构的支持但仍提供i386软件源（但没有i386内核， 即官方不再支持i386架构的机器），还彻底移除了对MIPS架构的支持。添加了对64位RISC-V架构的支持。  

注：Debian 12 (Bookworm)为最后一个支持i386架构的版本。  

## 解决Y2K38问题
除i386的软件外的其他架构的软件均已使用64位的`time_t`时间戳数据类型以避免Y2K38（2038年）问题。  

## 软件更新

- 内核从6.1系列更新到了6.12系列  
- glibc更新到了2.41  
- GCC更新到了14.2（如需安装gcc15见：[GCC-15](../hilevel/gcc15.md)）  
- systemd更新到了257  
- GNOME更新到了48  
- KDE更新到了6.3  

除此之外还有许多软件进行了更新，并移除了许多过时的软件添加了许多新的软件，以及部分软件的回归。  

## 内核及驱动
Linux内核从6.1更新到6.12，支持大部分20225夏季及之前出产/组装的计算机的基础硬件。  
NTFS文件系统驱动由以前的ntfs-3g（用户态驱动）更改为更高性能的ntfs3（内核态）驱动（但可能会引入一些小问题，见：[当前Debian的部分bugs](../start/bugs.md)）。  

## GNOME的更新
GNOME42到GNOME48经历了6次更新，其中包含了许多各种各样的的更新。  

- GNOME48引入了动态三缓冲技术令GNOME拥有更加丝滑流畅的动画，还在多方面提升了GNOME桌面环境的性能并减少了资源的占用。    
- GNOME48添加了HDR显示的支持。  
- 许多GNOME组件及GTK应用更新到了GTK4框架，这带来了更加现代用户界面和交互体验。  
- GNOME48添加了通知折叠功能。  
- Nautilus变的更加现代化人性化。其移除了Other Locations侧栏项目，并允许更加自由的侧栏书签调整，还可以点击文件位置区域访问文件位置地址栏且可修改其中地址以快速跳转访问其他位置。  
- Gnome设置经历了重大的更新，其发生了许多变化且更加现代化了。在GNOME48中还添加了“健康”(Wellbeing)设置菜单，用于设置一个合理且健康的计算机使用安排。  
- 更新了Quick Settings菜单（原system stauts menu）让其更强大具有更多功能。  
- 原来左上角的Activities（活动）按钮现已变更为Activities Indicator（活动指示器）按钮，其可用于标示当前所在的工作区，其仍然保留了打开Activities Overview的功能。  
除此之外还有许多更新未在此列出，如需了解详细gnome更新相关信息可见“参考资料”的对应条目。  

## 参考资料

\[1\] [What’s new in Debian 13](https://www.debian.org/releases/trixie/release-notes/whats-new.en.html)  
\[2\] [Introducing GNOME 48, “Bengaluru”](https://release.gnome.org/48/)  
\[3\] [Introducing GNOME 47, “Denver”](https://release.gnome.org/47/)  
\[4\] [Introducing GNOME 46, “Kathmandu”](https://release.gnome.org/46/)  
\[5\] [Introducing GNOME 45, “Rīga”](https://release.gnome.org/45/)  
\[6\] [Introducing GNOME 44, “Kuala Lumpur”](https://release.gnome.org/44/)  
\[7\] [Introducing GNOME 43, “Guadalajara”](https://release.gnome.org/43/)  

---
Author: smgdream | License: CC BY-NC-SA | Version: 0.6.1 | Date: 2025-10-23

