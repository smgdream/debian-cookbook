# Debian系统升级

## 在线升级

在开始升级之前首先要[备份系统](sys-backup.md)。然后将Debian apt源配置文件中的套件名(suite)中出现的旧版本Debian套件名更改为新版本Debian套件名（如，将bookworm更改为trixie，将bookworm-updates更改为trixie-updates）。  

如果当前存在单独安装的独显驱动则需要登出桌面环境，然后以root用户登录纯终端界面，结束显示管理(DM)进程并卸载独显驱动（如，之前安装了NVIDIA官方驱动，则通过命令`nvidia-uninstall`卸载驱动，如果是通过apt安装的由Debian源仓库提供的NVIDIA驱动，则通过apt卸载有关软件包），之后重启计算机。  

然后登出桌面环境并以root用户登录纯终端界面结束显示管理(DM)进程。接下来执行以下命令更新软件包列表并升级系统。  
```sh
apt update
apt full-upgrade
```
升级完毕后重启计算机。  

进入到新版本系统后执行`apt autoremove`命令卸载无用软件包。apt可能会输出一些类似“以下软件没有进行更新：xxx xxx xxx ...”的信息。对应这些软件可以通过执行以下命令进行更新。  
```sh
apt reinstall xxx xxx xxxx ...
apt autoremove
```

然后执行命令`aptitude purge "~c"`清除已被卸载的软件的配置文件。之后，如果有需要则重新[安装独显驱动](graphics-card.md)。  

最后，重启计算机即可完成Debian系统的升级。  

## 离线升级

首先需要[备份系统](sys-backup.md)并卸载单独安装的独显驱动（如果有的话），然后参考[APT 离线更新软件](../hilevel/apt-update-offline.md)的“使用iso映像离线升级Debian Bookworm为Debian Trixie”段落的步骤离线升级Debian。

## 参考资料

\[1\][Upgrades from Debian 12 (bookworm)](https://www.debian.org/releases/trixie/release-notes/upgrading.en.html)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5 | Date: 2025-08-04
