# Debian cookbook

**Debian食用手册**  
[License: CC BY-NC-SA 4.0](./LICENSE)  
Version: 0.9.5 Dev
**注意：此版本为开发版，部分内容尚未更新或完善**
## 1 适用范围

本手册旨在提供适用于Debian系统的使用说明和配置说明，该手册适合有简单Linux基础的人阅读。  

限制：本手册内容适用于Debian 13，因本人能力所限，当前手册桌面环境相关内容主要局限于GNOME桌面环境。  

## 2 微社区
如果有关于使用Debian和其他Linux相关的的问题或有关于本手册的建议与可在[Discussions](https://github.com/smgdream/debian-cookbook/discussions)讨论区进行讨论和说明，如果要提交勘误和改进建议请提交issue。  

## 3 贡献说明

本人欢迎各位为本手册添砖加瓦，但必须以CC BY-SA 4.0协议或CC0协议发布。而且本手册要求所有内容不得使用AI生成并且需自然人贡献者验证并保证内容的有效性，内容本身也不得违法或侵权。<!-- 至于为什么贡献者贡献的文章的允许的发布协议不包含CC BY-NC-SA是因为在多人合作中该协议会带来各种各样难以处理的问题所以目前只允许笔者本人使用该协议 -->  

## 4 内容索引

<!-- intro -->
> 0. 何为Debian  
> [本手册的食用方法](intro/eat.md)  
> [何为Debian](intro/debian.md)  
> [Debian Trixie的新特性](intro/trixie.md)  
> [Debian的硬件兼容性](intro/deb-hcp.md)  
> [我与Debian](intro/linux-and-mine.md)  

<!-- start -->
> 1. 启程  
> [当前Debian适合我吗](start/deb-suit4me.md)  
> [Debian安装](start/install-deb.md)  
> [Debian初步配置](start/init-conf.md)  
> [Debian基本环境配置](start/base-conf.md)  
> [Gnome基本桌面环境配置](start/desktop-conf.md)  
> [基本开发环境配置](start/dev-conf.md)  
> [不访看看Debian官方资料](start/deb-docs.md)  
> [当前Debian的部分bugs](start/bugs.md)  

<!-- improve -->
> 2. 进阶  
> [Debian系统备份](improve/sys-backup.md)  
> [Debian+windows](improve/deb+win.md)  
> [Debian对各家显卡的支持](improve/graphics-card.md)  
> [NVIDIA驱动的安装与配置](improve/install-nv.md)  
> [NVIDIA驱动使用wayland显示服务](improve/nv-wayland.md)  
> [Gnome调教与美化](improve/gnome-conf.md)  
> [动态壁纸](improve/live-wp.md)  
> [Debian多媒体源](improve/deb-mmedia.md)  
> [Linux多媒体工作站](improve/media-workstation.md)  
> [国产软件的支持说明](improve/cn-software.md)  
> [Wine](improve/wine.md)    
> [Linux游戏](improve/linux-game.md)  
> [Debian系统升级](improve/deb-full-upgrade.md)  
> [在Debian中配置打印机](improve/deb-printer.md)  
> [Backports源的配置与使用](improve/backports.md)  
> [笔者的Debian初始化配置](improve/mydeb-oobe.md)  

<!-- hilevel -->
> 3. 高阶  
> [APT离线更新软件](hilevel/apt-update-offline.md)  
> [Debian To Go](hilevel/deb-togo.md)  
> [安装GCC 15](hilevel/gcc-15.md)  
> [Desktop启动器](hilevel/desktop.md)  
> [自定义文件缩略图生成](hilevel/thumbnail.md)  

<!-- usage -->
> 4. 软件使用简要教程  
> [无法退出的VIM](usage/vim-q.md)  
> [配置VIM](usage/vim-conf.md)  
> [输入法介绍](usage/im-intro.md)  
> [中州韻輸入法 RIME](usage/rime.md)  
> [雾凇拼音 rime-ice](usage/rime-ice.md)  
> [其他软件使用说明](usage/other.md)  
> [部分软件的运行方法](usage/exec.md)  

<!-- where -->
> 5. 官方资料何处寻  
> [Debian官网简单指引](where/deb-web-index.md)  
> [Debian用户社区](where/deb-com.md)  
> [Debian软件包相关信息](where/pkg-info.md)  
> [Linux Wiki](where/linux-wiki.md)  
> [部分开源软件官网及ftp](where/software-web.md)  
> [其他资料源](where/other-doc.md)  

<!-- compile -->
> 6. 软件编译专题  
> [Linux](compile/linux.md)  
> [musl libc](compile/musl.md)  
> [GCC](compile/gcc.md)  
> [tcc](compile/tcc.md)  
> [SDL3](compile/sdl3.md)  
> [Make构建系统的使用方法]()  

<!-- miscellanea -->
> 7. 知识杂物房  
> [Debian季报](misc/deb-news.md)  
> [系统急救包](misc/os-aid.md)  
> [过时命令的代替品](misc/new-cmd.md)  
> [各apt软件源介绍](misc/apt-repo.md)  
> [Appimage食用说明](misc/appimage.md)  
> [安全漏洞追踪](misc/cves.md)  
> [固件更新](misc/fwupd.md)  
> [日志的查看与管理](misc/logs.md)  
> [Manual的阅读说明](misc/rdfm.md)
<!-- > 构建Mine Tiny Linux -->  

> 8. 书籍推荐  
<!-- > The C Programming Language -->   
<!-- > UNIX adv env -->  

## 协议说明
*Debian Cookbook*在下文中将其称作“本手册”。  

本手册目前包含的文章采用CC BY-NC-SA 4.0协议、CC BY-SA协议及CC0 1.0协议发布，使用者必须遵循相关协议的要求。每篇文章的协议均写在文章章末，如果存在未说明协议的文章则默认其使用CC BY-SA 4.0协议。在部分文章中可能使用了第三方的媒体文件（插图、视频等），对于这些媒体内容则遵循其原始许可约束进行再分发（这些媒体内容不受其所在文章协议的约束）。

## 相关仓库
**[debian-file](https://github.com/smgdream/debian-file)**: 跟debian-cookbook相关的一些代码、脚本、软件、文件的存储仓库。  
