# Debian对各家显卡的支持
当前Debian可以支持NVIDIA, AMD, Intel的显卡。其中对AMD和Intel的核显支持较好但对其独显的支持有所欠缺，而arm平台Soc中的显卡因为过于封闭故支持较差。下面对常见各家显卡驱动的安装方法进行简要说明。  

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

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.2 | Date: 2026-01-01
