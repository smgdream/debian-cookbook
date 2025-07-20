# APT 离线更新软件

在部分环境中我们 不能/不想 在线 更新/安装 软件，于是我们需要本地源来更新软件。  
使用`cdrom`型URI有时会出现莫名其妙的错误导致无法更新软件故在此不做讨论（注意：这并不代表我们无法使用光碟或iso映像进行本地软件更新），本文目前就`file`型URI进行讨论。  

如果对应本地源已具有正确的密钥以及合适的有效时间配置，则直接使用以下配置即可  
one-line-style:  
```
deb file://ABSOLUTE_PATH SUITE [COMPONENT1] [COMPONENT2] [...]

example:
deb file:///mnt/mydisc/offlinesrc/13 trixie contrib main non-free non-free-firmware
```
deb822-style:  
```
Types: deb
URIs: file:ABSOLUTE_PATH
Suites: SUITE
Components: [COMPONENT1] [COMPONENT2] [...]
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

example:
Types: deb
URIs: file:/mnt/mydisc/offlinesrc/13
Suites: trixie
Components: contrib main non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
```

但是如果缺少密钥则要使用`Trusted`选项来信任软件源。如果有效时间配置存在问题则需要通过`Check-Valid-Until`选项来忽略其有效时间配置（当然断网修改系统时间也是一种方法）。  
one-line-style:  
```
deb [trusted=yes check-valid-until=no] file://ABSOLUTE_PATH SUITE [COMPONENT1] [COMPONENT2] [...]

example:
deb [trusted=yes check-valid-until=no] file:///mnt/mydisc/offlinesrc/13 trixie contrib main non-free non-free-firmware
```
在one-line格式中，如存在多个选项则用空格分隔  

deb822-style:  
```
Types: deb
URIs: file:ABSOLUTE_PATH
Suites: SUITE
Components: [COMPONENT1] [COMPONENT2] [...]
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
Trusted: yes
Check-Valid-Until: no

example:
Types: deb
URIs: file:/mnt/mydisc/offlinesrc/13
Suites: trixie
Components: contrib main non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
Trusted: yes
Check-Valid-Until: no
```
根据实际情况来看deb822似乎没有要求注意选项关键词的大小写。  
`Trusted: yes`表示信任该源故不再检查密钥，`Check-Valid-Until: no`表示关闭对该源的有效时间检查。尽管在以上两个示例中同时使用了两个选项，但部分实际环境中只使用其中一个选项就可以了。  

## 实践

### 使用iso映像离线升级Debian Bookworm为Debian Trixie

如果当前正在使用桌面环境要登出桌面环境，然后使用root账户登录tty，如果有桌面管理（DM）服务正在运行需停止该服务（如：`systemctl stop gdm3`）。  

然后挂载iso镜像  
```sh
mkdir /mnt/debian
mount YOUR_ISO_FILE /mnt/debian
```
然后将当前所有软件源作无效化处理并增添以下源配置  
one-line-style:
```
deb [trusted=yes check-valid-until=no] file:///mnt/debian trixie contrib main non-free-firmware
```
deb822-style:
```
Types: deb
URIs: file:/mnt/debian
Suites: trixie
Components: contrib main non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
Trusted: yes
Check-Valid-Until: no
```
在以上两种格式中，如果是其他系统版本需要根据实际情况更改Suites字段的值（即光碟中dists目录中的目录名，根据需求选一个），还需要根据实际情况设置Components字段的内容（通常为即光碟中dists/SUITES/目录中的目录名）。  

最后通过以下命令更新源数据并更新软件  
```sh
apt update
apt upgrade
```
<br>

在iso映像中的软件更新可能无法覆盖到当前系统上的所有软件，这种情况下需要在通过iso映像更新完主要软件后设置在线软件源以进一步更新软件。更新完软件后可通过`apt autoremove`卸载不再需要的软件（该命令在个别情况下会卸载系统仍然需要的软件，使用需慎重）。  

更新系统相关的资料见：[Upgrades from Debian 12 (bookworm)](https://www.debian.org/releases/trixie/release-notes/upgrading.en.html)


## 参考资料
\[1\][sources.list(5) - Debian Manpages](https://manpages.debian.org/man/5/sources.list)  
\[2\][Upgrades from Debian 12 (bookworm)](https://www.debian.org/releases/trixie/release-notes/upgrading.en.html)

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.8 | Date: 2025-07-14
