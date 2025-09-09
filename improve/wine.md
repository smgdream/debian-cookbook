# Wine

Wine全称"Wine is not an emulator"(Wine不是一个模拟器)，它是一个用于在UNXI系统上运行Windows应用程序的兼容层，它通过将Windows API调用转换为POXIS API调用来使得Windows应用程序得以在UNIX系统中运行。它是Linux用户在Linux系统上使用Windows应用程序的必不可少的工具。通过Wine我们可以在Linux上运行Windows应用程序甚至还可以以较高性能运行不少游戏。

注意：Wine并不能完美运行所有Windows应用程序，适配较好的程序可以正常运行，适配较差的程序则运行出现明显问题或无法运行。  

## 安装Wine

### 安装官方Wine

注：如果因为地区网络问题导致无法正常安装wine请将下文出现的所有`https://dl.winehq.org/wine-builds/`替换为wine镜像站的URL（如：`https://mirror.nju.edu.cn/wine-builds/`）

使用官方wine可以及时体验最新的稳定版和测试版。安装方法如下。  

除非你确定不会运行32位windows程序，否则运行以下命令启用32位架构支持：  
```sh
dpkg --add-architecture i386
apt update
```
然后安装仓库密钥：  
```sh
wget -O - https://dl.winehq.org/wine-builds/winehq.key | gpg --dearmor -o /etc/apt/keyrings/winehq-archive.key
```

之后再添加wine源仓库。  
```sh
wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/debian/dists/trixie/winehq-trixie.sources
```
注：如果当前系统版本不是Debian Trixie则要将以上命令中的所有`trixie`替换为对应系统版本的发行名。如果使用镜像站，部分镜像源仓库可能没有根据该镜像源另外修改sources文件的URIs字段的值，此时则需要将sources文件中URIs字段值中的`https://dl.winehq.org/wine-builds/`替换为镜像站的URL（如：`https://mirror.nju.edu.cn/wine-builds/`）。  

然后通过`apt update`更新软件包列表，之后可执行以下命令安装一个合适的Wine分支。  
| Wine分支 | 安装命令 |
| --- | --- |
| 稳定分支 | `apt install --install-recommends winehq-stable` |
| 开发分支 | `apt install --install-recommends winehq-devel` |
| Staging 分支 | `apt install --install-recommends winehq-staging` |

对于没启用32位架构支持的用户则通过以下方法安装wine。

首先安装需要的分支：  
| Wine分支 | 安装命令 |
| --- | --- |
| 稳定分支 | `apt install --install-recommends wine-stable-amd64` |
| 开发分支 | `apt install --install-recommends wine-devel-amd64` |
| Staging 分支 | `apt install --install-recommends wine-staging-amd64` |

然后手动在[wine源仓库](https://dl.winehq.org/wine-builds/debian/pool/main/w/wine/)下载以下两个文件`wine-BRANCHNAME_VERSION~DISTNAME-1_amd64.deb`和`winehq-BRANCHNAME_VERSION~DISTNAME-1_amd64.deb`，之后手动将这两个包的内容解压到相应目录。  
部分命令示例（仅供参考）  
```sh
# wine
cp -rv ./opt/wine-stable/* /opt/wine-stable/

# winehq
cp -rv ./bin/* /usr/local/bin/

# optional
rm /usr/local/bin/wine
ln -s /usr/local/bin/wine64 /usr/local/bin/wine
```

### 通过Debian源安装wine
除非你确定不会运行32位windows程序，否则运行以下命令启用32位架构支持：  
```sh
dpkg --add-architecture i386
apt update
```
然后使用以下命令安装wine。  
```sh
apt install wine
```

## 安装wine Mono和Gecko
Wine Mono用于让wine支持运行.NET Framework 应用程序，Gecko是wine独立实现的一个Internet Explorer。它们的安装方法如下：  

### 安装wine Mono
首先查询下表确认所需安装的Wine Mono版本（请务必按照表格的说明安装对应的版本）。  
| Wine 版本 | Wine Mono 版本 |
| --- | --- |
| 10.10 | 10.1.0 |
| 10.5 | 10.0.0 |
| 10.0-rc1 | 9.4.0 |
| 9.17 | 9.3.0 |
| 9.12 | 9.2.0 |
| 9.8 | 9.1.0 |
| 9.2 | 9.0.0 |
| 8.19 | 8.1.0 |
| 8.9 | 8.0.0 |
| 7.20 | 7.4.0 |

注：完整表格见[Wine Mono](https://gitlab.winehq.org/wine/wine/-/wikis/Wine-Mono#versions)  

然后前往[Wine Mono下载页](https://dl.winehq.org/wine/wine-mono/)下载合适的wine mono 版本。其中提供了tar包和msi两种格式的安装包，tar包用于为所有Wine全局安装Mono（安装完毕后创建的wine实例才有mono），msi包用于为已创建的wine实例单独安装Mono。这两种包的安装方式如下：  

**安装tar包**  
将对应Mono安装包下载到本地后通过以下命令安装Mono：  
```sh
mkdir -pv /opt/wine/mono/
tar -xvf PATH_TO_MONO_TARBALL -C /opt/wine/mono/
```
安装完后新建的wine实例中将包含mono。

**安装msi包**  
将对应Mono安装包下载到本地后通过以下命令为当前wine实例安装Mono：  
```sh
wine msiexec /i PATH_TO_MONO_MSI
```

### 安装Gecko

首先查询下表确认需要安装的Gecko版本（请务必按照表格的说明安装对应的版本）。  
| Wine版本范围 | Gecko版本 |
| --- | --- |
| wine-7.13 - wine-8.5 | 2.47.3 |
| wine-8.6 - 当前最新版本 | 2.47.4 |

注：完整表格见[Gecko](https://gitlab.winehq.org/wine/wine/-/wikis/Gecko#installing)  

然后前往[Gecko下载页](https://dl.winehq.org/wine/wine-gecko/)下载合适版本的Gecko版本。其中提供了两种架构`x86`和`x86_64`的Gecko构建，如果开启了32位架构支持或确定当前Wine构建支持WoW64则两种架构都需要安装，否则只需安装`x86_64`架构的Gecko。Gecko也提供了tar包和msi包两种安装包，tar包用于为所有Wine全局安装Gecko（安装完毕后创建的wine实例才有Gecko），msi包用于为已创建的wine实例单独安装Gecko。这两种包的安装方法如下：  

**安装tar包**  
将Gecko安装包下载到本地后执行以下命令安装Gecko。  
```sh
mkdir -pv /opt/wine/gecko/
tar -xvf PATH_TO_GECKO_TARBALL -C /opt/wine/gecko/
```
安装完后新建的wine实例中将包含Gecko。  

**安装MSI包**  
将Gecko安装包下载到本地后执行以下命令为当前Wine实例安装Gecko。  
```sh
wine msiexec /i PATH_TO_GECKO_MSI
```
## 安装中文字体
安装部分软件依赖于专门的中文字体，如果不安装中文字体，文本将显示为方框。中文字体的安装方法如下：  

将命名为`msyh.ttc`的微软雅黑字体文件复制到wine安装目录下的`share/wine/fonts`目录中即可。安装完后新建的wine示例将包含该字体。

[微软雅黑字体下载](pool/msyh.ttc)

<!-- vxdk 3d11? -->

## wine 简单使用说明

使用wine运行程序

```sh
wine PROGRAM [ARGS...]
```

使用wine运行程序（不显示debug信息）
```sh
WINEDEBUG=-all wine PROGRAM [ARGS...]
```

wine实例的设置
```sh
winecfg
```

wine实例内的软件安装卸载器
```sh
wine uninstaller
```

wine实例内的控制面板
```sh
wine control
```

wine实例内的资源管理器
```sh
wine explorer
```

wine实例内的注册表
```sh
wine regedit
```

可通过设置WINEPREFIX环境/临时变量来指定/设置wine实例目录的位置如：  
设置为环境变量。  
```sh
exprot WINEPREFIX=~/.wine-game
wine PROGRAM [ARGS...]
```
设置为临时变量。  
```sh
WINEPREFIX=~/.wine-game wine PROGRAM [ARGS...]
```

## Wine版本使用报告
经本人测试Wine 10.0存在较多bug，许多应用无法正常运行（部分游戏运行出错，甚至从源代码编译安装的wine无法运行）。而当前最新开发版Wine 10.12运行windows应用的问题少得多。

## Wine游戏
见[Linux游戏](linux-game.md)的“Wine游戏”段落。  

## 其他Wine相关信息
有关Wine运行Windows应用的兼容情况可通过[Wine AppDB](https://appdb.winehq.org/)查询。  

## 参考资料

\[1\] [WineHQ](https://www.winehq.org)  
\[2\] [Inatall wine for Debian and Ubuntu - Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/Debian-Ubuntu)  
\[3\] [Wine - Debian Wiki](https://wiki.debian.org/Wine#Standard_installation)  
\[4\] [Wine Mono - Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/Wine-Mono)  
\[5\] [Gecko - Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/Gecko)  
\[6\] [FAQ - Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/FAQ)  
\[7\] [Wine builds 软件仓库镜像使用帮助](https://help.mirrors.cernet.edu.cn/wine-builds/)  
\[8\] [Wine User's Guide  Wine - Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/Wine-User's-Guide)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.8 | Date: 2025-08-21
