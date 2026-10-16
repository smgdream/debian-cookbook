# Wine
<!-- 未来需根据新的Wow64 Wine构建对本文做一次全面的内容调整 -->
<!-- 根据新发布的wine11调整本文内容 -->
<!-- 基本调整已完成，但仍有待进一步的调整 -->

Wine全称"Wine is not an emulator"(Wine不是一个模拟器)，它是一个用于在UNXI系统上运行Windows应用程序的兼容层，它通过将Windows API调用转换为POXIS API调用来使得Windows应用程序得以在UNIX系统中运行。它是Linux用户在Linux系统上使用Windows应用程序的必不可少的工具。通过Wine我们可以在Linux上运行Windows应用程序甚至还可以以较高性能运行不少游戏。  

在64位环境中Wine可以运行64位、32位和16位Windows程序。没错Wine现在还能在64位环境中运行16位程序，这可是连Microsoft自己都做不到的黑科技，该功能需要使用wine 10.16及更新版本。  

注意：Wine并不能完美运行所有Windows应用程序，适配较好的程序可以正常运行，适配较差的程序则运行出现明显问题或无法运行。  

## 安装Wine

### 安装官方Wine

注：如果因为地区网络问题导致无法正常安装wine请将下文出现的所有`https://dl.winehq.org/wine-builds/`替换为wine镜像站的URL（如：`https://mirror.nju.edu.cn/wine-builds/`）

使用官方wine可以及时体验最新的稳定版和测试版。安装方法如下。  

对于Debian Trixie及之前版本或Ubuntu 25.04及之前版本，应当执行以下命令启用32位架构支持：  
```sh
dpkg --add-architecture i386
apt update
```
对于Debian Forky及未来版本或Ubuntu 25.10及更新版本则无须执行以上命令添加32位软件源。因为针对这些新系统构建的wine 10.18及更新版本使用了新的WoW64实现，其不再依赖32位运行时。

注：经笔者测试Debian Trixie可以安装供Debian Forky使用的Wine 11.0以避免需要启用i386库并安装i386依赖，安装方法和下文描述的wine安装方法别无二致，但是要把当前系统当作forky来执行本段落所说明的安装方法。

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

注1：稳定分支不一定稳定且少Bug，只意味着其稳定和少Bug的可能性更高。  

注2：本文为了跟随新wine的变化，已移除了**没启用32位架构支持**的wine安装方法对于有需要的用户请查看本手册0.8版本的此篇文章。  

根据[wine官方的新消息](https://gitlab.winehq.org/wine/wine/-/wikis/Debian-Ubuntu)：自wine 10.18开始将为Debian forky（当前的Debian Testing）和Ubuntu 25.10及更新版本提供包含新WoW64构建的Wine，不再分别需要64位和32位wien构建。以后安装wine就不需要添加i386源了。  

~~吐槽：当今Linux几乎所有软件都有64位了，如果不是为了使用Wine，我才不愿意添加i386源。真希望wine什么时候官方打包能变为WoW64的发布，到时候就能完全和32位say goodbye了。~~  


### 通过Debian源安装wine
首先应当执行以下命令启用32位架构支持：  
```sh
dpkg --add-architecture i386
apt update
```
然后通过以下命令安装wine。  
```sh
apt install wine
```
**注意：** 在Debian Trixie中的Wine版本为10.0，该版本虽说是稳定版但实际上既不稳定也不好用。  

## 安装wine Mono和Gecko
Wine Mono用于让wine支持运行.NET Framework 应用程序，Gecko是wine独立实现的一个Internet Explorer。它们的安装方法如下：  

### 安装wine Mono
首先查询下表确认所需安装的Wine Mono版本（请务必按照表格的说明安装对应的版本）。  
| Wine 版本 | Wine Mono 版本 |
| --- | --- |
| 11.16 | 11.3.0 |
| 11.12 | 11.2.0 |
| 11.8 | 11.1.0 |
| 11.3 | 11.0.0 |
| 11.0-rc4 | 10.4.1 |
| 11.0-rc1 | 10.4.0 |
| 10.17 | 10.3.0 |
| 10.14 | 10.2.0 |
| 10.10 | 10.1.0 |
| 10.5 | 10.0.0 |
| 10.0-rc1 | 9.4.0 |
| 9.17 | 9.3.0 |
| 9.12 | 9.2.0 |
| 9.8 | 9.1.0 |
| 9.2 | 9.0.0 |
| 8.19 | 8.1.0 |

注：对于介于两个表项之间的版本则向下“取整”至最接近的版本，如10.18“取整”到10.17其需安装mono 10.3.0，而对于11.0“取整”到11.0-rc4，其需安装mono 10.4.1。完整表格见[Wine Mono](https://gitlab.winehq.org/wine/wine/-/wikis/Wine-Mono#versions)  

然后前往[Wine Mono下载页](https://dl.winehq.org/wine/wine-mono/)下载合适的wine mono 版本。其中提供了tar包和msi两种格式的安装包，tar包用于为所有Wine全局安装Mono（安装完毕后创建的wine实例才有mono），msi包用于为已创建的wine实例单独安装Mono。这两种包的安装方式如下：  

**安装tar包**  
将对应Mono安装包下载到本地后通过以下命令安装Mono：  
```sh
mkdir -pv /opt/wine/mono/
tar -xvf </PATH/TO/MONO_TARBALL> -C /opt/wine/mono/
```
安装完后新建的wine实例中将包含mono。

**安装msi包**  
将对应Mono安装包下载到本地后通过以下命令为当前wine实例安装Mono：  
```sh
wine msiexec /i </PATH/TO/MONO_MSI>
```

### 安装Gecko

首先查询下表确认需要安装的Gecko版本（请务必按照表格的说明安装对应的版本）。  
| Wine版本范围 | Gecko版本 |
| --- | --- |
| wine-8.6 - 当前最新版本 | 2.47.4 |
| wine-7.13 - wine-8.5 | 2.47.3 |

注：完整表格见[Gecko](https://gitlab.winehq.org/wine/wine/-/wikis/Gecko#installing)  

然后前往[Gecko下载页](https://dl.winehq.org/wine/wine-gecko/)下载合适版本的Gecko版本。其中提供了两种架构`x86`和`x86_64`的Gecko构建，如果开启了32位架构支持或确定当前Wine构建支持WoW64则两种架构都需要安装，否则只需安装`x86_64`架构的Gecko。Gecko也提供了tar包和msi包两种安装包，tar包用于为所有Wine全局安装Gecko（安装完毕后创建的wine实例才有Gecko），msi包用于为已创建的wine实例单独安装Gecko。这两种包的安装方法如下：  

**安装tar包**  
将Gecko安装包下载到本地后执行以下命令安装Gecko。  
```sh
mkdir -pv /opt/wine/gecko/
tar -xvf </PATH/TO/GECKO_TARBALL> -C /opt/wine/gecko/
```
安装完后各wine实例即可使用Gecko。  

注：以该方式安装的Gecko在使用时wine会直接从UNIX安装中加载Gecko，而非在创建实例时将其安装在实例中，故在uninstaller中是看不到Gecko相关的软件包的。  

**安装MSI包**  
将Gecko安装包下载到本地后执行以下命令为当前Wine实例安装Gecko。  
```sh
wine msiexec /i </PATH/TO/GECKO_MSI>
```
## 安装中文字体
安装部分软件依赖于专门的中文字体，如果不安装中文字体，文本将显示为方框。中文字体的安装方法如下：  

将命名为`msyh.ttc`的[微软雅黑字体文件](../pool/msyh.ttc)复制到wine安装目录下的`share/wine/fonts`目录中即可。安装完后新建的wine实例将包含该字体。  

## wine半离线安装
注：此内容适用于提供的Debian系统的wine 10.18及更新的WoW64版本（不包含staging分支）。

在安装wine时虽然wine的软件包是离线的，但是依赖仍需要在线安装，这也就是本段落为什么被称为《wine半离线安装》的原因。  

离线安装wine仅需下载两个软件包，分别为`winehq-<branch>_<version>~<suit>-1_amd64.deb`和`wine-<branch>_<version>~<suit>-1_amd64.deb`这两个文件均位于`https://dl.winehq.org/wine-builds/debian/pool/main/w/wine/`目录中。

说明：  
- `<branch>`: wine的分支名，有效分支名分别为`stable`（稳定分支）、`devel`（开发版分支）、`staging`（特性尝鲜分支）。  
- `<version>`: wine的版本。
- `<suti>`: 相应系统版本的套件名，即系统代号，如`trixie`或`forky`。
  
> 如对于适用于Debian Forky的Wine 11.0则需要下载[winehq-stable_11.0.0.0~forky-1_amd64.deb](https://dl.winehq.org/wine-builds/debian/pool/main/w/wine/winehq-stable_11.0.0.0~forky-1_amd64.deb)和[wine-stable_11.0.0.0~forky-1_amd64.deb](https://dl.winehq.org/wine-builds/debian/pool/main/w/wine/wine-stable_11.0.0.0~forky-1_amd64.deb)。  

然后通过apt安装这两个软件包即可（安装过程通常需要网络来安装wine的依赖）。  

## wine 简单使用说明

使用wine运行程序

```sh
wine <PROGRAM> [ARGS...]
```

使用wine运行程序（不显示所有fixme debug信息）
```sh
WINEDEBUG=fixme-all wine <PROGRAM> [ARGS...]
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

wine实例内置的扫雷
```sh
wine winemine
```

安装msi安装包
```sh
wine msiexec /i <MSI_FILE>
```

可通过设置`WINEPREFIX`环境/临时变量来指定/设置wine实例目录的位置，如：  

设置为临时环境变量  
```sh
exprot WINEPREFIX=<WINE_INSTANCE_PATH>
wine <PROGRAM> [ARGS...]
```
设置为临时变量  
```sh
WINEPREFIX=~/.wine-game wine <PROGRAM> [ARGS...]
```
如果没有设置`WINEPREFIX`变量则wine实例的默认路径为`~/.wine`。  

创建32位架构的wine实例  
```sh
WINEPREFIX=<WINE32_INSTANCE_PATH> WINEARCH=win32 wineboot -i
```
说明：  
- `WINEPREFIX`变量只是为了另外指定一个wine实例目录以避免和现有wine实例目录冲突。  
- `WINEARCH`变量用于指定wine实例的架构，其有效值为`win64`（默认）和`win32`，`win64`可运行64位程序以及32位程序（如果安装了32位wine的话），`win32`只能运行32位程序。对于wine 11.0及更新版本移除了值`win32`并添加了值`wow64`，`wow64`用以指明强制旧的WoW64安装以新的WoW64模式运行。<!-- wow64具体的功能是什么仍不清楚，仅wine 11.0的release中提了一句，目前wiki中仍没有相关内容，但根据字面意思该值基本用不到。 -->  
- `wineboot -i`命令用于创建并初始化一个wine实例，另外`wineboot -u`命令可用于更新相应的wine实例。

## Wine版本可用性报告
经笔者实际测试Wine 10.0存在较多bug，许多应用无法正常运行（部分游戏运行出错，甚至从源代码编译安装的wine无法运行）。目前建议使用最新的稳定版Wine 11.0。

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
\[8\] [Wine User's Guide - Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/Wine-User's-Guide)  
\[9\] [Commands - Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/Commands)  
\[10\] [msiexec - Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/Commands/msiexec)  
\[11\] [wineboot - Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/Commands/wineboot)  
\[12\] [Download- Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/Download)  
\[13\] [WINEDEBUG channels - Wine User's Guide - Wine Wiki](https://gitlab.winehq.org/wine/wine/-/wikis/Wine-User's-Guide#winedebug-channels)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.7.2u | Date: 2026-06-17
