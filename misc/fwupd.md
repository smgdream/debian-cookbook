# 更新固件
<!-- 待调整段落编排 -->

固件是用于控制的电气设备的嵌入式软件，其既可以提前烧录在设备的ROM或Flash中，也可以在设备初始化时加载到设备的存储器中。有时候厂商会为其设备提供新的固件，新的固件通常用于改进设备性能、扩展设备功能、修复设备Bug。所以如果厂商为设备提供了新固件，通常都建议更新固件以获得更好的体验。

[Linux Firmware](https://gitlab.com/kernel-firmware/linux-firmware)是一个和Linux内核一起分发的固件包（在Debian中，其依据厂商和设备类型被拆分为多个包）。所以当Linux Firmware获得了更新，如果更新的包适用于计算机的设备，设备的固件也自然会由此获得更新。除了Linux Firmware之外还有一些其他的固件包，在安装新版的固件包时就会更新相应的固件。

在Linux中我们还会使用[fwupd](https://github.com/fwupd/fwupd)来更新固件，fwupd是一个用于更新设备固件的守护进程。fwupd中包含了一个名为fwupdmgr的程序用于管理固件更新。

## 使用fwupdmgr在线更新固件
首先通过`fwupdmgr refresh`命令从[LVFS](https://fwupd.org/)（Linux Vendor Firmware Service，Linux供应商固件服务）在线获取固件元数据。然后可以通过`fwupdmgr get-updates`命令查看当前系统中可以更新固件的设备。如果存在可升级的固件可以通过`fwupdmgr update`命令从LVFS下载并安装新固件。  

注：上文中的命令中出现的`fwupdmgr`亦可以替换为`fwupdtool`。fwupdtool是一个独立的固件更新工具，其可以直接使用fwupd插件<!-- 接口？ -->，而无需守护进程。fwupdtool相比fwupdmgr还额外提供了一些功能。  

除此之外我们还可以使用gnome-firmware（固件）、gnome-software(GNOME软件商店)、plasma-discover (KDE Discover)这几个图形化软件来与fwupd通信并更新固件。

## 附加信息

### UEFI安全启动证书更新！！！
2013版UEFI安全启动证书将于UTC时间2026年10月19日中午过期。如果你的计算机需要使用安全启动功能且UEFI固件中的证书尚未更新至2025版，请尽快更新UEFI固件中的证书。更新方法可参考上文的“使用fwupdmgr在线更新固件”段落。  

## 参考资料
\[1\] [fwupd - ArchWiki](https://wiki.archlinux.org/title/Fwupd)  
\[2\] fwupdmgr(1)  
\[3\] fwupdtool(1)  
\[4\] [LVFS: Home](https://fwupd.org/)  
\[5\] [LVFS: Users](https://fwupd.org/lvfs/docs/users)  
\[6\] [LVFS: Hardware Vendors](https://fwupd.org/lvfs/docs/vendors)  
\[7\] [LVFS: Developers](https://fwupd.org/lvfs/docs/developers)  
\[8\] [Linux Vendor Firmware Service — LVFS  documentation](https://lvfs.readthedocs.io/en/latest/index.html)  
\[9\] [Firmware/Updates - Debian Wiki](https://wiki.debian.org/Firmware/Updates)  
\[10\] [SecureBoot/CAChanges - Debian Wiki](https://wiki.debian.org/SecureBoot/CAChanges)  
\[11\] [Linux firmware - ArchWiki](https://wiki.archlinux.org/title/Linux_firmware)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.4 | Date: 2026-10-16