# Debian多媒体源

Debian有一个非官方但由Debian开发者长期维护的第三方多媒体源`deb-multimedia`，该源提供了许多多媒体软件的最新版本，还提供了一些Debian官方源中没有收录的多媒体软件。因为该多媒体源由Debian开发者维护所以不用担心该源稳定性以及与官方Debian源的兼容问题。  

## 配置deb-multimedia源

新建文件`/etc/apt/sources.list.d/dmo.sources`并在其中写入以下源配置。  
```
Types: deb
URIs: https://www.deb-multimedia.org
Suites: trixie
Components: main non-free
Signed-By: /usr/share/keyrings/deb-multimedia-keyring.pgp
```
注：如果因为地区网络问题导致更新/安装软件速度较慢，可以将`URIs`对应的值替换为其他镜像站的地址（如：`https://mirrors.cernet.edu.cn/deb-multimedia/`或`https://mirrors.ustc.edu.cn/deb-multimedia/`，URL不强求以斜杠结尾）。  

然后通过以下命令下载并安装密钥。  
```sh
wget https://www.deb-multimedia.org/pool/main/d/deb-multimedia-keyring/deb-multimedia-keyring_2024.9.1_all.deb
apt install ./deb-multimedia-keyring_2024.9.1_all.deb
rm ./deb-multimedia-keyring_2024.9.1_all.deb
```
注：对于国内用户可将第一行命令中的`https://www.deb-multimedia.org/`替换为其他镜像站的地址（如：`https://mirrors.cernet.edu.cn/deb-multimedia/`或`https://mirrors.ustc.edu.cn/deb-multimedia/`）  

最后更新软件列表并更新系统。  
```sh
apt update
apt dist-upgrade
```

## 参考资料

\[1\] [How to use the Debian multimedia repository](https://deb-multimedia.org/)  
\[2\] [Deb Multimedia - USTC Mirror Help](https://mirrors.ustc.edu.cn/help/deb-multimedia.html)  
\[3\] [Debian - Wikipedia](https://en.wikipedia.org/wiki/Debian)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.6.1 | Date: 2025-10-13
