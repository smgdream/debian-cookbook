# 过时命令的代替品

知识以媒体为媒介传播到千家万户，许多知识历久弥新，这些知识被人们称之为定理、真理或经典。在当今这个高速发展的时代各种事物日新月异，而为了迎上这日新月异的时代许多知识也如雨后春笋般出现。新时代不单止伴随着新事物的诞生亦伴随着旧事物的消逝。常常，一个旧事物式微了消逝了，但记载这这些旧事物的媒介仍然存在，许多时候这些媒介是对过去的念想是旧事物曾经存在的证明。但在这之下却潜藏的一个问题，记载着旧事物的媒介和记载了新事物的媒介夹杂在了一起，这给找寻新事物相关知识的人造成了麻烦——本应查找和新事物相关的资料却得到的是旧事物的资料，然后把这当作了新事物的资料，甚至进一步传播误导了更多后来人。  

在信息技术领域这类问题则更为突出。例如，当今C23都出了一年了而在各种媒介上的C语言相关内容还是ANSI C的C99相关的资料也只有零星几点，又如在前端网页开发这个高速发展的领域网上仍充斥着千禧年时的远古资料。而这类情况在中文媒介上犹为明显。在Linux命令相关的资料也存在这些问题，下面笔者将指出一些已经过时的命令，以及说明与这些旧命令具有相似功能的新命令。  

| 新命令 | 旧命令 | 说明 |
| --- | --- | --- |
| `ip` | `ifconfig`, `arp`, `route`, `ipmaddr`, `iptunnel` | ip工具用于替代以前多个独立的命令来显示或配置网络设备，如可以使用`ip a`命令替代`ifconfig`来查看各网络接口的ip。ip命令的使用方法可通过`man ip`命令查看。 |
| `ss` | `netstat` | ss用于代替netstat查看套接字和网络连接信息。 |
| `fastfetch` | `neofetch` | neofetch已5年未更新且其已于2024年8月归档。该命令的代替品为使用C语言编写的fastfetch，其运行速度远超neofetch。且其支持众多操作系统，笔者建议使用fastfetch来代替neofetch显示系统信息。 |
| `xorriso` | `mkisofs`, `genisoimage` | `mkisofs`和`genisoimage`是两个功能基本一致的经典的iso映像制作光盘刻录工具，它们已经十几年没更新了，对新一些的光盘格式支持较差，故不再推荐使用该这两个工具制作iso映像或刻录光盘。现在建议使用`xorriso`工具来制作iso映像或刻录光盘，该工具支持新标准的光盘文件系统及新标准的蓝光光碟映像。其至今仍在缓慢更新。xorriso的使用较为复杂，可通过`man xorriso`命令查看其手册。 |

## 参考资料
\[1\] IP(8)  
\[2\] [genisoimage - Debian Wiki](https://wiki.debian.org/genisoimage)  
\[3\] [Index of /gnu/xorriso](https://ftp.gnu.org/gnu/xorriso/)  
\[4\] [一图总结linux上正在被替换的命令！！！](https://mp.weixin.qq.com/s/w-1yGo9SNafzIZlzF84Uwg)  
\[5\] ss(8)  
\[6\] netstat(8)  
\[7\] Debian Reference v2026 Section 5.5.1 Iproute2 commands

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.3.4 | Date: 2026-10-16