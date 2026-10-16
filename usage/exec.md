# 部分软件的运行方法
本部分软件因为某些原因无法正常运行，下面笔者将说明如何修复然后正常运行这些软件。  

## 常见的Appimage运行修复方法
因为沙盒问题导致的appimage无法运行（常见于国产软件的appimage软件分发）：运行appimage时添加`--no-sandbox`参数。  

## VirtualBox
注意：VirtualBox在Wayland中运行时存在bug，如果虚拟机全屏运行时点击屏幕下边的虚拟机控制栏的控件将无法进行任何控制并会导致光标失效（在X11下运行则没有此问题）。另外安装VirtualBox请使用相应系统的构建不要使用​All distributions通用构建，否则VirtualBox虚拟机可能无法正常启动。    

对于Linux内核版本为6.16及以上的Linux系统使用VirtualBox 7.2.2及更新版本可正常使用VirtualBox，但无法同时使用VirtualBox虚拟机和KVM虚拟机。  

对于版本为6.12.x到6.15.x的Linux内核处境就有点尴尬了。VirtualBox从7.2.0某一小版本开始提供了对Debian Trixie的支持，我们可以正常安装并新建设置虚拟机。但你将发现无法启动虚拟机，这是因为从Linux 6.12开始，当机器启动内核加载KVM时将会自动启用虚拟化（通过`kvm.enable_virt_at_load`功能），这将会独占系统的虚拟化特性，导致无法使用别的虚拟化软件。故需要通过一些方法来关闭kvm虚拟化才能使用VirtualBox，方法如下：  

### 方法一  
在使用虚拟机前执行以下命令关闭kmv模块以释放虚拟化资源。  
**Intel CPU的机器**  
```sh
modprobe -r kvm_intel kvm
```
**AMD CPU的机器**  
```sh
modprobe -r kvm_amd kvm
```

后续可通过重启计算机或执行以下命令重新启用KVM。  

**Intel CPU的机器**  
```sh
modprobe kvm_intel nested=1
modprobe kvm
```
**AMD CPU的机器**  
```sh
modprobe kvm_amd nested=1
modprobe kvm
```

### 方法二  
设置内核命令行参数或添加modprobe配置以关闭`kvm.enable_virt_at_load`功能，相应两种子方法如下（以下的法一法二选其一即可）:  

**法一 设置内核启动参数**  
创建配置文件`/etc/default/grub.d/disable-kvm-virt-at-load.cfg`并在其中写入以下内容。  
```
GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX kvm.enable_virt_at_load=0"
```
然后通过`update-grub`命令更新grub启动配置文件。  

**法二 添加modeset配置**  
创建配置文件`/etc/modprobe.d/disable-kvm-virt-at-load.conf`并在其中写入以下内容并保存。  
```
options kvm enable_virt_at_load=0
```

## NVIDIA驱动与Blender的兼容性问题
NVIDIA 580驱动和590驱动对Blender存在兼容性问题，当NVIDIA 驱动为相应大版本的驱动时使用Blender极易发生崩溃，且会将NVIDIA显卡从总线上拔出。鉴于此问题不建议使用以上版本驱动。请使用其他版本的NVIDIA显卡驱动，如575。

## 旧版本Krita Appimagae 解压运行
我们可以直接运行Krita Appimage，在5.3及更新版本中将appimage解压也能正常运行。但对于5.2及之前的几个版本，如果试图将appimage解压运行Krita却无法启动了。其实更早之前的版本是可以正常解压运行的，但在某次更新之后就不行了。  

通常如果一个Appimage无法解压运行则要修改其AppRun脚本。但是Krita的AppRun不是一个脚本而是一个可执行文件，这下就麻烦了。但因为笔者本人的曾经需要，通过一段时间的运行日志分析，最终编写出了以下可正常启动Krita的AppRun脚本。
```sh
#!/bin/sh

this_dir="$(dirname "$0")"
export PYTHONPATH=""$this_dir"/usr/lib/krita-python-libs/krita"
export PYTHONHOME=""$this_dir"/usr"
exec "$this_dir"/usr/bin/krita
```
解压后无法运行的问题出在krita的运行需要其专门的python库。所以要设置相应环境变量，将python相关路径变量设置为krita自带的python的路径才能运行。（但笔者仍不明白为什么appimage解压前可以正常运行，解压后就无法运行）  

## GDM Settings
GDM Setting是一个用于设置gdm的工具我们可以用其设置gdm的背景、主题、光标等内容，但从其[官方github页面](https://github.com/gdm-settings/gdm-settings)下载的appimage可能因为动态链接库的符号问题而无法直接运行，通过配置使其正常运行的方法如下:  

首先赋予appimage文件可执行权限并通过`--appimage-extract`参数解压该appimage，然后在其中AppRun脚本中的程序执行语句之前添加一行`export LD_PRELOAD=/lib/x86_64-linux-gnu/libc.so.6`，随后保存即可执行AppRun脚本运行GDM Setting。  

## 钉钉
见：[国产软件](../improve/cn-software.md)中钉钉支持情况的附加信息的内容。  

## QQ Music
见：[国产软件](../improve/cn-soutware.md)中qq music支持情况的附加信息的内容。

## 参考资料

\[1\] [[Bug]: Unsupported resolution for screen shot: 0x0 (screen 0) #144](https://github.com/VirtualBox/virtualbox/issues/144)  
\[2\] [[Req]: Please set kvm.enable_virt_at_load=0 in /etc/modprobe.d #188](https://github.com/VirtualBox/virtualbox/issues/188)  
\[3\] [Trouble starting VB - disable KVM](https://forums.virtualbox.org/viewtopic.php?t=112955)  
\[4\] [#22248 closed defect (wontfix) Trouble with loaded module "kvm" starting a VM under Kernel 6.12](https://www.virtualbox.org/ticket/22248)  
\[5\] [Linux_6.12 - Linux Kernel Newbies](https://kernelnewbies.org/Linux_6.12)  
\[6\] [Linux 6.12 - Linus Torvalds](https://lore.kernel.org/all/CAHk-=wgtGkHshfvaAe_O2ntnFBH3EprNk1juieLmjcF2HBwBgQ@mail.gmail.com/)  
\[7\] [[PATCH v3 8/8] KVM: Enable virtualization at load/initialization by default - Sean Christopherson](https://lore.kernel.org/all/20240608000639.3295768-9-seanjc@google.com/)  
\[8\] [KVM: Add a module param to allow enabling virtualization when KVM is loaded](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b4886fab6fb620b96ad7eeefb9801c42dfa91741)  
\[9\] [Re: KVM default behavior change on module loading in kernel 6.12 - by Sean Christopherson](https://lore.kernel.org/kvm/ZwQjUSOle6sWARsr@google.com/)  
\[10\] [Re: KVM default behavior change on module loading in kernel 6.12 - by Paolo Bonzini](https://lore.kernel.org/kvm/CABgObfYWZQc_gnzUAmFQ=McbN4VQxbrd+4vss=pGRdrOAcOcfg@mail.gmail.com/)  
\[11\] [Changelog for VirtualBox 7.2.2](https://www.virtualbox.org/wiki/Changelog-7.2#v2)  
\[12\] [[Bug]: Linux host VERR_SVM_IN_USE after update kernel to 6.14 #81](https://github.com/VirtualBox/virtualbox/issues/81)  
\[13\] [[Bug]: VMs fail to start with critical error on Linux hosts with Kernel 6.12+ due to KVM module conflict #141](https://github.com/VirtualBox/virtualbox/issues/141)  
\[14\] [KVM - ArchWiki](https://wiki.archlinux.org/title/KVM)  
\[15\] [symbol lookup error: /tmp/.mount_GDM_SercdIqA/usr/lib/libc.so.6: undefined symbol: __tunable_is_initialized, version GLIBC_PRIVATE #264](https://github.com/gdm-settings/gdm-settings/issues/264)  
\[16\] [Zero-dependency AppImage support | Current state of the AppImage #209](https://github.com/gdm-settings/gdm-settings/issues/209)  
\[17\] [Use a new libc.so.6 with LD_PRELOAD](https://forums.opensuse.org/t/use-a-new-libc-so-6-with-ld-preload/162548)  
\[18\] [[Bug]: CTRL+F and then selecting menu at bottom of screen looses mouse in VM · Issue #322](https://github.com/VirtualBox/virtualbox/issues/322)  
\[19\] [virtualbox stops responding to mouse input. Linux Wayland host #4](https://github.com/VirtualBox/virtualbox/issues/4)  
\[20\] [Wayland Support of VirtualBox Host Software? - virtualbox.org](https://forums.virtualbox.org/viewtopic.php?t=110046)  
\[21\] [23 January 2026 - YouTube](https://www.youtube.com/watch?v=YFx2Z1v1GSc)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.8u | Date: 2026-04-22
