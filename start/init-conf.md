# Debian初步配置
以下配置仅供参考，可根据需求选择性配置。  

## root用户语言设置
root用户下无法正常显示ASCII字符以外的字符，这是因为root用户的默认语言为`C`所致。  
配置方法：手动将`/root/.profile`中的`LANG`和`LANGUAGE`变量对应的值从`C`更改为`C.UTF-8`。  

## 对当前用户启用sudo
只有在sudo用户组中的用户才可以使用sudo命令，而Debian安装程序不会将其创建的普通用户添加到sudo用户组。  
配置方法：以root用户执行`usermod -a -G sudo USER_NAME`，然后彻底登出用户或重启计算机。  

## 设置用户目录下的文件夹为英文
如果安装Debian的默认语言是中文，则用户目录下的“文档”、“音乐”、“图片”、“视频”等文件夹名会被设置为中文名。而部分软件对中文路径的支持存在问题，所以用户目录下的文件夹为中文名可能会导致部分软件出错。因此建议将用户目录下的文件夹名设置为英文。  
配置方法：要修改目录名的用户执行以下命令  
```sh
LANGUAGE=C xdg-user-dirs-update --force
```

## 重新配置语言环境
配置方法：执行以下命令  
```sh
dpkg-reconfigure locales
```
确保在语言编译列表中所需的语言已被勾选并确定，然后选择系统语言即可。（注：可以通过该方法将系统语言设为英文，然后在GNOME设置->系统->区域与语言中将“语言”设置为“汉语(中国)”，以实现系统/终端语言为英文，GUI程序语言为中文的效果）  

## 配置常规软件源
Debian很多时候安装软件都通过apt从网络源中下载并安装软件，而想要这么做得预先配置好软件源。  
配置方法：新建`/etc/apt/source.list.d/debian.sources`然后写入以下内容并保存。  
```
Types: deb
URIs: https://deb.debian.org/debian/
Suites: trixie trixie-updates
Components: main contrib non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
```
注：发行版名称（`Suites`字段）可能要依据实际需求稍作修改。如果因为地区网络问题导致更新/安装软件速度较慢可以将URIs对应的字段替换为[其它镜像站](https://www.debian.org/mirror/list)的地址（其URL不用以斜杠结尾）（如：https://mirrors.cernet.edu.cn/debian/或https://mirrors.ustc.edu.cn/debian/）。  
然后通过`apt update`命令更新档案库元数据。  

## 配置安全源
及时获取安全更新修复系统漏洞是极为重要的事，而想要及时获得安全修复更新则要配置好安全源。  
注：在配置安全源之前请先配置好常规软件源。  
配置方法：在`/etc/apt/source.list.d/debian.sources`添加以下内容并保存。  
```
Types: deb
URIs: https://security.debian.org/debian-security/
Suites: trixie-security
Components: main contrib non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
```
注：发行版名称(`Suites`)可能要依据实际需求稍作修改。如果因为地区网络问题导致更新/安装软件速度较慢可以将`URIs`对应的值替换为其它镜像站的地址（其URL不要求以斜杠结尾）（如：https://mirrors.cernet.edu.cn/debian-security/或https://mirrors.ustc.edu.cn/debian-security/）。因为镜像站的更新存在延迟，所以对于有及时修复安全漏洞需求的用户应使用官方安全源，以确保能及时获得安全更新。  
然后通过`apt update`命令更新档案库元数据。  

## 参考资料
\[1\] [XDG user directories - ArchWiki](https://wiki.archlinux.org/title/XDG_user_directories)  
\[2\] [SourcesList - Debian Wiki](https://wiki.debian.org/SourcesList)  
\[3\] [StableUpdates - Debian Wiki](https://wiki.debian.org/StableUpdates)  
\[4\] [Chapter 2. Debian package management - Debian Reference](https://www.debian.org/doc/manuals/debian-reference/ch02.en.html)  
\[5\] [Debian - USTC Mirror Help](https://mirrors.ustc.edu.cn/help/debian.html)  
\[6\] [Debian Security - USTC Mirror Help](https://mirrors.ustc.edu.cn/help/debian-security.html)  
\[7\] [Debian 软件仓库镜像使用帮助 - 校园网联合镜像站](https://help.mirrors.cernet.edu.cn/debian/)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.7.2 | Date: 2025-07-24
