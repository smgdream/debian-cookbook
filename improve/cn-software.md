# 国产软件

常用国产软件支持情况如下  

<!-- √ -->
| 软件名 | 原生支持 | Wine支持 | 附加信息 |
| :--- | :---: | :---: | --- |
| [QQ](https://im.qq.com/linuxqq/index.shtml) | √ | - | - |
| [微信](https://linux.weixin.qq.com/) | √ | - | 官方提供的deb包存在bug可能会破坏正常的系统配置文件（非致命）。因程序组件逻辑设计问题可能存在无故遍历磁盘文件的"BUG"导致占用较多的磁盘IO资源。第一个bug可以通过使用Appimage格式的微信解决，安装[flathub版微信](https://flathub.org/zh-Hans/apps/com.tencent.WeChat)可以解决这两个BUG。 |
| [WPS](https://www.wps.cn/product/wpslinux) | √ | - | - |
| [腾讯会议](https://meeting.tencent.com/download/) | √ | - | - |
| [钉钉](https://www.dingtalk.com/download) | √ | - | 存在轻度功能阉割 |
| [QQ音乐](https://y.qq.com/download/download.html) | √ | - | 存在核心功能阉割（无法下载音乐.） |
| TIM | X | X | - |

题外话：关于微信linux版很多人的认为它是2024年才推出的，但实际上微信开发团队早在2021年就推出了微信linux（仅有基本的聊天功能和文件收发功能）。但下载链接不在微信官网而在优麒麟(Ubuntu Kylin)的应用下载网页中。在这三年中每次看到有人说linux没有微信就无语一次。  
![WeChat linux 2.1.1](images/ukl-wechat.jpeg)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5 | Date: 2025-08-01
