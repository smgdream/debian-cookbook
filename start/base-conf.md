# Debian基本配置
以下内容仅供参考，请依据需求选择性配置。  

## 常用软件安装
可通过`apt install PACKAGE1_NAME PACKAGE2_NAME ...`命令安装一个或多个软件。  
以下程序列表中的程序根据建议以及实际情况安装。  
| 包名 | 描述 |
| --- | --- |
| aptitude | apt团队开发的一个基于apt的软件包管理器（建议安装） |
| vim | 极为常用且强大的文本编辑器（建议安装） |
| ranger | 控制台文件管理器 |
| fastfetch | 系统信息展示工具（建议安装） |
| htop | 交互式进程监视器 |
| btop | 强大的交互式进程监视器 |
| nvtop | GPU监视器 |
| iotop | io占用查看器 |
| ncdu | 磁盘占用查看器 |
| curl | 通过URL传输数据的命令行工具（建议安装） |
| wget | 极为常用的下载器（建议安装） |
| wget2 | wget的重写版，支持多线程等现代特性 |
| aria2 | 强大的高速下载器，还支持bt资源下载 |
| timeshift | 系统备份工具（建议安装） |
| grub-efi-amd64 | GRUB EFI引导写入器 |
| gdisk | GPT磁盘分区工具 |
| ffmpeg | 一个被广泛使用的强大的多媒体处理工具 |
| lynx | 终端网络浏览器 |

## 常用库安装
可通过`apt install PACKAGE1_NAME PACKAGE2_NAME ...`命令安装一个或多个软件。  
以下库根据实际需要安装。  
| 包名 | 说明 |
| --- | --- |
| libfuse2t64 | appimage运行依赖库（在Debian trixie中需要安装该库才能直接运行appimage程序，否则只能解压运行） |

## 授予普通用户写freambuffer权限
部分终端软件可能需要写freambuffer权限，而普通用户默认没有写framebuffer权限，所以要通过以下命令授予写framebuffer权限：  
```sh
usermod -a -G video USER_NAME
```
说明：将用户添加至video组中即可获得写framebuffer权限。  

## 时间同步
注意：Debian trixie已默认安装systemd-timesyncd用于时间同步，故无须执行该步骤。

对于Debian Bookworm及以前版本需要手动安装NTP程序以进行数据同步，安装并启用NTP服务的命令如下：  
```sh
apt install ntpsec
systemctl enable ntpsec
systemctl start ntpsec
```

## 笔记本设置
用于笔记本电脑的额外设置。  

### 笔记本低电量行为设置
通过设置合理的笔记本低电量行为可以保护笔记本电池。  
设置方法：  
首先，编辑`/etc/UPower/UPower.conf`文件，设置`PercentageLow`（低电量）、`PercentageCritical`（严重不足电量）、`PercentageAction`（采取行为电量）对应的值（值对应电量百分比，值的大小要求：`PercentageLow >= PercentageCritical >= PercentageAction`）。然后将`CriticalPowerAction`的值设置为`PowerOff`（以在电池电量降低到`PercentageAction`指定值时关闭计算机）。最后通过`systemctl restart upower`命令重启upower服务或重启计算机即可应用电池配置。  

### 笔记本合上盖子行为设置
Debian中默认笔记本合上盖子的行为为挂起计算机，而有时我们需要将其设置为其他行为（如：无动作）。配置方法如下：  
编辑`/etc/systemd/logind.conf`文件，取消`HandleLidSwitch`行的`#`注释。任何将`HandleLidSwitch`的对应值修改为合上笔记本盖子所需的行为（如：ignore，无操作）。最后重启计算机应用配置（注意：请勿相信网上说的重启`systemd-logind`服务的操作，否则可能导致桌面环境卡死）。  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.6.6 | Date: 2025-10-03
