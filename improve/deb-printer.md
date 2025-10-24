# 在Debian中配置打印机
打印机是一个比较常用的外设。下文将说明任何在Linux中配置打印机。  

有一小部分打印机在Linux中可以即插即用，对于这类打印机没有什么需要说明的。部分打印机类似即插即用但需要另外安装一个ppd文件，可在网上寻找对应打印机的ppd文件并在系统设置的打印机设置安装ppd文件。在gnome的打印机设置中通过*查看更多->打印机详细信息->安装PPD文件*即可安装打印机ppd配置。  

## EPSON
对于EPSON打印机在[EPSON Linux驱动下载页](https://download.ebz.epson.net/dsc/search/01/search/?OSC=LX)中输入产品型号。在随后出现的搜索结果中“模块名称”中有"Printer Driver"的即为打印机驱动程序（部分打印机附带扫描仪则“类别”为"Scanner Driver"的即为扫描仪驱动），点击对应的下载按钮，然后在接下来的页面中同意协议，之后点击Package Download Page即可进入真正的驱动下载页面，随后点击deb包对应的Download即可下载驱动。  

然后通过`apt install /path/to/printer_driver.deb`命令来安装驱动，随后即可正常连接使用打印机。  

闲话：如果在驱动下载页面好好看看你还会发现，这打印机驱动竟然还开源了。部分日本厂商对Linux用户还真挺友善的。  

## HP
对于HP打印机通过以下命令安装驱动程序。  
```
apt install hplip python3-pyqt5
```
注：可选安装`hplip-gui`  

然后连接打印机，并以普通用户身份执行`hp-setup`初始化打印机并配置驱动。随后即可使用打印机。  

注：部分打印机在安装过程中需要安装hplip plugin，hp-setup可以自动在线安装hplip plugin。但在部分地区因为网络问题无法安装plugin则需要先关闭hp-setup，然后在[HP plugins - openprinting](https://www.openprinting.org/download/printdriver/auxfiles/HP/plugins/)下载相应版本的`hplip-VERSION-plugin.run`（版本即为hplip的版本，可通过`apt list hplip`命令查看）。随后授予plugin可执行权限并以普通用户身份安装。之后重新执行`hp-setup`就能正常初始化打印机并配置驱动了。  

## 参考资料

\[1\] [HP plugins - openprinting](https://www.openprinting.org/download/printdriver/auxfiles/HP/plugins/)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.3.3 | Date: 2025-10-10
