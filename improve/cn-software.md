# 国产软件

常用国产软件支持情况如下  

<!-- √ -->
| 软件名 | 原生支持 | Wine支持(v10.12) | 附加信息 |
| :--- | :---: | :---: | --- |
| [QQ](https://im.qq.com/linuxqq/index.shtml) | √ | - | - |
| [微信](https://linux.weixin.qq.com/) | √ | - | 官方提供的deb包存在bug可能会破坏正常的系统配置文件（非致命）。因程序组件逻辑设计问题可能存在无故遍历磁盘文件的BUG导致占用较多的磁盘IO资源。第一个bug可以通过使用Appimage格式的微信解决，安装[flatpak版微信](https://flathub.org/zh-Hans/apps/com.tencent.WeChat)可以解决这两个BUG。 |
| [WPS](https://www.wps.cn/product/wpslinux) | √ | - | 特别说明：WPS从12.1.0之后的某个版本(<= 12.1.2)开始默认的新建模板（及保存）文件的格式变为了WPS的特有格式，这可能会导致办公文件在其他办公软件上存在兼容性问题，请务必注意！如果想使用微软格式的办公文件，请分别新建空白文档、表格、幻灯片并**以微软格式**（docx、xlsx、pptx）保存到`~/Templates`目录中，之后即可在右键新建文档菜单中新建微软格式的办公文件。在日常使用时请额外注意办公文件的保存格式，当然也可以一劳永逸的在WPS**各办公组件**的 *文件->选项->常规与保存->保存->文件保存默认格式* 中，将相应的办公文件默认保存格式设置为微软格式。<br>注：另外为了保证安全建议将添加到`~/Templates`目录中的各微软办公文件的所有者/组更改为root（通过chown命令）并将文件权限更改为全员只读（通过chmod命令）。<br>吐槽：这事微软负一半责任，故意把办公文件格式规范复杂化，给其他办公软件使绊子，造成办公文件格式的分化。另一半便是信创的要求。但目前微软格式仍是唯一的通用办公文件格式，也只能暂时先继续用下去了。 |
| [腾讯会议](https://meeting.tencent.com/download/) | √ | - | - |
| [钉钉](https://www.dingtalk.com/download) | Δ | - | 存在轻度功能阉割<br> 注：钉钉对Debian Trixie的支持存在问题无法直接运行。<br> 解决方法：下载[execstack](http://archive.ubuntu.com/ubuntu/pool/universe/p/prelink/execstack_0.0.20131005-1.1ubuntu1_amd64.deb)并安装，然后执行命令`execstack -c '/opt/apps/com.alibabainc.dingtalk/files/<VER>/dingtalk_dll.so'`（`<VER>`与钉钉版本号相关，不同钉钉版本的目录名有所不同，请根据实际情况修改命令中的该目录名称），之后即可正常启动钉钉。另外钉钉的deb包控制文件中的依赖配置可能没有写全，如果遇到该情况，可查看终端的库缺失信息并安装相关的依赖库。 |
| [迅雷](https://archive2.kylinos.cn/DEB/KYLIN_DEB/pool/all/com.xunlei.download_1.0.0.5_amd64.deb) | √ | Δ | 需安装依赖`libdbus-glib-1-2`，Linux版存在核心功能阉割（无法使用迅雷云盘相关功能） |
| [QQ音乐](https://y.qq.com/download/download.html) | √ | ΔΔ | Linux版存在核心功能阉割（无法下载音乐）。<br> QQ音乐原生Linux可能无法直接运行，需要添加`--no-sandbox`运行参数。(这个bug已经相当久没修复了，其他一样是electron开发的应用不加`--no-sandbox`也能跑，怎么就QQ音乐就不行了？盯~ 注重权限管理且对硬盘占用不敏感的用户可出门左转[flatpak qqmusic](https://flathub.org/zh-Hans/apps/com.qq.QQmusic))<br> 如果要在wine中运行QQ音乐建议使用安装版（而非便携版），否则可能会出现各种各样的bug，导致难以使用QQ音乐。 |
| TIM | X | - | - |

注：`√`完美支持，`Δ`存在问题（`Δ`轻微问题基本不影响使用，`ΔΔ`有些问题勉强能用，`ΔΔΔ`存在严重问题仅能启动），`X`不支持，`-`略。

题外话：关于微信linux版很多人的认为它是2024年才推出的，但实际上微信开发团队早在2021年就推出了微信linux（仅有基本的聊天功能和文件收发功能）。但下载链接不在微信官网而在优麒麟(Ubuntu Kylin)的应用下载网页中。在这三年中每次看到有人说linux没有微信就无语一次（但凡多搜一下，多看点资料都能找到这个版本）。  
![WeChat linux 2.1.1](images/ukl-wechat.jpeg)  

## 参考资料

\[1\] [debian13钉钉无法启动](https://forums.debiancn.org/t/topic/5796)  
\[2\] [linux版qq音乐安装后无法打开解决办法](https://www.bilibili.com/opus/962937083753660424)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.6.3u | Date: 2026-06-26
