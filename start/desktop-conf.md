# Debian基本桌面环境配置
以下配置仅供参考，请根据实际情况选择性配置。  

## 没什么用的GUI程序卸载
可通过`apt remove <PACKAGE_NAME > ...`命令卸载程序。  
可根据需求卸载以下少用的GUI程序。  
| 包名 | 说明 |
| --- | --- |
| baobab | 磁盘空间使用分析器 |
| evince | 文档查看器 |
| gnome-calculator | gnome计算器 |
| gnome-calendar | gnome日历（将其卸载不会影响顶栏的日期时间功能） |
| gnome-characters | gnome表情包查看器 |
| gnome-clocks | gnome时钟（将其卸载不会影响顶栏的日期时间功能） |
| gnome-contacts | gnome联系人 |
| gnome-maps | gnome地图 |
| gnome-music | gnome音乐 |
| gnome-snapshot | gnome相机 |
| gnome-sound-recorder | gnome录音机 |
| gnome-tour | gnome导览 |
| gnome-weather | gnome天气 |
| goldendict* | 黄金词典 |
| libreoffice-* | Libreoffice办公套件 |
| malcontent | 家长控制 |
| shotwell | gnome相片管理器 |
| simple-scan | gnome文档扫描 |
| totem | gnome媒体播放器（卸载该软件会导致部分媒体文件缩略图无法生成，有关该问题的修补见：[自定义文件缩略图生成](../hilevel/thumbnail.md)） |
| yelp | gnome帮助 |

卸载完毕后建议使用`apt autoremove`命令卸载依赖，然后使用`aptitude purge "~c"`命令移除已卸载软件的配置文件。  

## 安装有用的GUI程序
可通过`apt install <PACKAGE_NAME > ...`命令安装一个或多个软件。  
以下程序列表中的程序根据实际情况安装。  
| 包名 | 描述 |
| --- | --- |
| ptyxis | 现代化的gtk4终端模拟器（可用于取代gnome-terminal） |
| gnome-tweaks | Gnome高级设置（用于设置部分Gnome Setting无法设置的内容，如：最大最小化按钮的显隐），关于其使用方法可见[Gnome的调教与美化](../improve/gnome-conf.md) |
| gnome-shell-extensions | gnome拓展管理器 |
| dconf-editor | 图形化的dconf编辑器 |
| gparted | 图形化的分区工具 |
| vlc | 强大的媒体播放器 |
| nomacs | 多功能图片查看器 |
| deadbeef | 专业本地音乐播放器（需前往[DeaDBeeF官网](https://deadbeef.sourceforge.io/)下载deb包并安装） |
| helvum | 图形化pipewire音频路由控制器 |
| missioncenter | 现代化的gtk4系统监视器（需通过[flathub安装](https://flathub.org/apps/io.missioncenter.MissionCenter)或使用[GitLab发布页提供](https://gitlab.com/mission-center-devs/mission-center/-/releases)的appimage构建，如果安装了nvtop其将启用显卡占用显示功能 |
| motrix  | 基于aria2的通用下载器（需前往[Motrix官网](https://motrix.app/)下载） |
| qbittorrent | 著名的bittorrent客户端，用于下载bt资源 |
| k3b | Linux中功能强大的光盘烧录软件，可烧录CD、DVD、BD并制作光盘映像文件（但是对中文光盘卷名的支持存在问题，且不支持超过10个汉字的卷名）。一名为Package Forge (Dev)的组织构建了[k3b的Appimage](https://github.com/pkgforge-dev/K3b-AppImage)包 |

## Nautilus没有Other Locations书签的相关问题

注：Nautilus即GNOME桌面环境默认的“文件”应用。  

Nautilus 47移除了Other Locations（其他位置）侧栏项。其中内部硬盘项与网络项全部移至侧栏，但是根目录项却没了。这对部分用户造成了不便。  

如果需要一个快捷的根目录入口，可以将根目录以书签的形式添加至侧栏中。在地址栏输入`/`并回车即可跳转至根目录。然后可通过`Ctrl+D`快捷键或点击地址栏右侧的三个点再点击“添加书签”即可添加书签至侧栏。如有需要还可以右键侧栏的书签以重命名书签。  

## gnome界面的调教与美化
关于此部分内容见：[Gnome的调教与美化](../improve/gnome-conf.md)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.6.4u | Date: 2026-10-16
