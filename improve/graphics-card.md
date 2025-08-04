# Debian对各家显卡的支持
当前Debian可以支持NVIDIA, AMD, Intel, 摩尔线程的显卡。其中对AMD和Intel的核显支持较好但对其独显的支持有所欠缺，而arm平台Soc中的显卡因为过于封闭支持较差。下面对各家显卡驱动的安装方法进行简要说明。  

## NVIDIA
关于NVIDIA显卡的驱动安装见[Deal late no more NVIDIA](install-nv.md)  

## AMD 
通过以下命令安装amd显卡开源驱动。  
```sh
apt install firmware-amd-graphics
```
## Intel
通过以下命令安装开源驱动。  
```sh
apt install firmware-intel-graphics
```

## 摩尔线程
在摩尔线程驱动中心选择对应显卡型号的适用于Ubuntu驱动并下载最新版本。然后通过以下命令安装驱动：  
```sh
apt install MUSA_DEB_PATH
```
注：MUSA_DEB_PATH为摩尔线程驱动deb包的路径，当路径为相对路径时路径需以`./`开头。  
**特别注意：当前摩尔线程显卡驱动尚未支持Debian Trixie**  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5 | Date: 2025-07-27
