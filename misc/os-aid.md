# 系统急救包

## REISUB
俗话说：“重启能解决90%的问题，……”，当Linux系统突然出现莫名其妙问题我们可能会尝试重启电脑，虽然我们可以通过电源键或restart键强制重启计算机，但毕竟强制重启可能会损坏数据消耗计算机寿命，所以在可以的情况下我们更倾向于先尝试软重启。  

在类似系统卡死的情况下我们可能无法通过命令或图形界面软重启计算机，但Linux内核还为我们提供了一种特殊的软重启方式 —— REISUB。REISUB是一个内核级的系统请求指令<sup>注1</sup>，只要内核没有卡死我们都可以通过该指令来重启系统。

要想使用REISUB需满足两个前置条件：1. 内核启用了CONFIG_MAGIC_SYSRQ编译选项 2. 当前系统已启用内核的sysrq功能对应的功能掩码。绝大部分发行版的内核默认已编译了相应功能，但部分系统可能没有完全启用<!-- 注意：措辞为“没有完全启用” -->sysrq对应的功能掩码。如需完全启用<!-- 注意：措辞为“完全启用” -->sysrq的全部指令可以使用`echo 1 >/proc/sys/kernel/sysrq`临时启用，也可以创建文件`/etc/sysctl.d/sysrq.conf`并在其中写入`kernel.sysrq = 1`然后重启计算机以永久启用。

### REISUB的触发
按住左Alt和SysRq键（如果键盘上没有独立的SysRq键，则该键通常print screen共用一个物理按键），然后分别按下`R` `E` `I` `S` `U` `B`即可<sup>注2</sup>（全程不要松开Alt和SysRq键）。

系统请求指令REISBU的功能如下表所示：

| 指令 | 功能 |
| - | - |
| r | 将键盘从RAW（扫描码）模式切换为XLATE（ASCII）模式 |
| e | 将`SIGTERM`信号发送给除`init`外的所有进程 |
| i | 将`SIGKILL`信号发送给除`init`外的所有进程 |
| s | 尝试同步(sync)所有已挂载的文件系统 |
| u | 尝试将所有已挂载的文件系统重新挂载为只读 |
| b | 立即重启系统（不去同步或卸载磁盘） |

注1：REISUB只是系统请求指令中的几个指令，还存在别的功能的其他系统请求指令，若对其他系统请求指令感兴趣可参见[Linux Magic System Request Key Hacks](https://docs.kernel.org/admin-guide/sysrq.html)。  

注2：每按完一个键后建议间隔3-5秒后再按下一个键，以让每个指令有充分的执行时间。执行S指令后建议根据系统中已挂载磁盘的写入速度，间隔10-120秒再按下一个键，以让数据得以全部写入磁盘。

## 参考资料
\[1\] [Linux Magic System Request Key Hacks](https://docs.kernel.org/admin-guide/sysrq.html)  
\[2\] [reisub使用方法-Renwen0524-ChinaUnix博客](http://blog.chinaunix.net/uid-8196371-id-4163118.html)  
\[3\] KBD_MODE(1)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.3 | Date: 2026-09-04
