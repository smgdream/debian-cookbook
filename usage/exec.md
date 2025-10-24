# 部分软件的运行方法
本部分软件因为某些原因无法正常运行，下面笔者将说明如何修复然后正常运行这些软件。  

## VirtualBox
注意：VirtualBox 7.2.2存在bug，如果虚拟机全屏运行时点击屏幕下边的虚拟机控制栏将无法进行任何控制并会导致光标失效。  

对于Linux内核版本为6.16及以上的Linux系统使用VirtualBox 7.2.2及更新版本可正常使用VirtualBox，但无法同时使用VirtualBox虚拟机和KVM虚拟机。  

对于版本为6.12.x到6.15.x的Linux内核处境就有点尴尬了。VirtualBox从7.2.0某一小版本开始提供了对Debian Trixie的支持，我们可以正常安装并新建设置虚拟机。但你将发现无法启动虚拟机，这是因为从Linux 6.12开始，当加载KVM时将会自动启用虚拟化（通过`kvm.enable_virt_at_load`功能），这将会独占系统的虚拟化特性，故需要通过一些方法来关闭kvm虚拟化才能使用VirtualBox，方法如下：  

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

## GDM Settings
GDM Setting是一个用于设置gdm的工具我们可以用其设置gdm的背景、主题、光标等内容，但从其[官方github页面](https://github.com/gdm-settings/gdm-settings)下载的appimage可能因为动态链接库的符号问题而无法直接运行，通过配置使其正常运行的方法如下:  

首先赋予appimage文件可执行权限并通过`--appimage-extract`参数解压该appimage，然后在其中AppRun脚本中的程序执行语句之前添加一行`export LD_PRELOAD=/lib/x86_64-linux-gnu/libc.so.6`，随后保存即可执行AppRun脚本运行GDM Setting。  

## 钉钉
见：[国产软件](../improve/cn-software.md)中钉钉支持情况的附加信息的内容。  

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

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.1 | Date: 2025-10-24
