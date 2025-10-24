# Linux游戏
如今在linux中可以游玩不少游戏了，不过和windows相比还是差了些。  

## 原生游戏

**Minecraft**  
说起原生支持linux的游戏，minecraft是绕不过的话题。minecraft使用java编写，所以只要目标平台能跑jvm它就能跑minecraft。在debian中运行mc的方法如下：  
首先需要安装jre（java runtime environment）。  
```sh
apt install openjdk-21-jre
```
然后下载一个mc启动器（如[hmcl](https://hmcl.huangyuhui.net/)）。  
之后即可通过java -jar hmcl.jar命令启动启动器，然后在启动器中下载mc并启动。  

**其他游戏**  
现在随着steam desk的发展和linux桌面用户的增长以及godot游戏开发者的增加。越来越多游戏加入了原生支持linux的队伍，如：ENDER LILIES、Half-life、BUCKSHOT ROULETTE等游戏。  

## Wine游戏

wine不单是为了运行windows应用，高性能地运行windows的游戏也是wine更新至今的一大目标。关于wine的安装方法和使用教程见[Wine](wine.md)。因为wine不是模拟器所以通过wine运行游戏性能损耗也较小，有的游戏甚至还比在windows上运行的帧率更高。  

通常使用命令`wine GAME_PROGRAM`即可通过Wine运行Windows游戏。在部分机器上Linux可能无法在wine运行游戏时自动将显卡设为高性能显卡，此时可以可以通过在程序运行命令前添加变量`__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only`即可指定使用nv独显渲染游戏（命令示例：`__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only wine GAME_PROGRAM`）。 

部分游戏在wine中可能需要在虚拟桌面环境中运行，否则游戏运行会存在一些问题。设置虚拟桌面环境的方法如下：  
方法一：执行命令winecfg,，在wine设置中勾选“图形”选项卡的“虚拟桌面”选项（永久性设置）。  
方法二：通过以下命令临时通过虚拟桌面环境运行游戏：  
```sh
wine explorer /desktop=name,WIDTHxHEIGHT GAME_PROGRAM
```
注：`WIDTH`为屏幕宽度像素数，`HEIGHT`为屏幕高度像素数。`name`为虚拟桌面名  

### 部分windows游戏在wine中运行的方法

注1：以下命令均假设你已将当前目录切换至游戏顶层目录。  
注2：以下版本要求仅代表经笔者测试可运行游戏的wine版本，而非最低wine版本要求。  

#### GRIS 
wine版本要求：>= 6.0  
运行方法：  
```sh
wine GRIS.exe
```
Bug: 在一些Wine版本中从主界面中退出游戏时可能会出现GC（垃圾回收）错误而无法结束关闭界面。强制结束游戏进程即可，不影响游戏存档。  

#### NieR:Automata（尼尔：机械纪元）
wine版本要求：>= 9.0  
运行方法：  
```sh
wine NieRAutomata.exe
```

#### Monument Valley I（纪念碑谷  I）
wine版本要求：>= 6.0  
运行方法：  
```sh
wine 'Monument Valley.exe'
```
第一次执行游戏时如果无法进入游戏游玩界面则强制结束游戏进程并再次启动。  

#### Monument Valley II（纪念碑谷  II）
wine版本要求：>= 7.0  
运行方法：  
```sh
wine 'Monument Valley 2.exe'
```
第一次执行游戏时如果无法进入游戏游玩界面则强制结束游戏进程并再次启动。  

#### Monument Valley III（纪念碑谷  III） 
wine版本要求：>= 10.0  
运行方法：  
```sh
wine 'Monument Valley 3.exe'
```

#### ENDER LILIES（终焉的莉莉丝）
wine版本要求：>= 9.0  
运行方法：  
```sh
wine EnderLilies.exe
```
第一次执行游戏时可能会弹出运行库安装界面，安装完运行库后即可正常运行游戏。  
Bug1：在部分场景中游戏角色可能因为wine渲染bug而缺少角色贴图，角色将被渲染成橙色剪影。  
Bug2：通过intel集显运行游戏其内容可能出现严重的渲染错误，如果拥有NVIDIA独显可以尝试通过下面的命令以NVIDIA独显运行游戏也许可以解决渲染错误的问题。  
```sh
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only wine EnderLilies.exe
```

#### ENDER MAGNOLIA（终焉的玛格诺利亚）
wine版本要求：>= 10.0  
运行方法：  
首先通过以下命令安装运行库。  
```sh
wine Engine/Extras/Redist/en-us/UEPrereqSetup_x64.exe
```
然后通过以下命令运行游戏。  
```sh
wine EnderMagnolia.exe
```
注3：在wine 10.0中无法通过`EnderMagnolia.exe`启动游戏，需通过以下命令运行游戏。  
```sh
wine EnderMagnolia/Binaries/Win64/EnderMagnoliaSteam-Win64-Shipping.exe
```  
注4：在部分环境中也许通过以上的方法仍无法运行游戏，而如果机器有NVIDIA独显则可以尝试通过以下命令运行游戏。
```sh
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only wine EnderMagnolia/Binaries/Win64/EnderMagnoliaSteam-Win64-Shipping.exe
```

#### Journey（风之旅人）
wine版本要求：>= 9.0  
运行方法：  
通过以下命令启动wine explorer：  
```sh
wine explorer
```
然后在explorer中打开游戏目录并双击运行游戏。  

#### Steins;Gate Steam Edition（命运石之门 Steam版）
wine版本要求：>=10.12  
运行方法：
通过以下命令创建32位wine实例
```sh
WINEPREFIX=~/.wine32 WINEARCH=win32 wineboot -i
echo initfinish
```
然后下载DirectX 9.0c并为该wine实例安装。

> 如通过第三方打包的[运行库ScKu](../pool/Scku.exe)安装DirectX 9.0c  
> `WINEPREFIX=~/.wine32 WINEARCH=win32 wine Scku.exe`  
> 然后选择Custom install（自定义安装），之后仅选择DirectX 9.0c并安装即可。  

如果Steins;Gate为非英文版还需要安装相应语言的字体，如对于中文版则需要将微软雅黑的字体文件msyh.ttc复制到实例的C:\windows\Fonts中。

之后通过以下命令运行explorer
```sh
WINEPREFIX=~/.wine32 WINEARCH=win32 wine explorer
```
最后通过explorer启动游戏启动器`Launcher.exe`并点击开始游戏即可游玩。



## Steam

Steam早已原生支持linux，而Valve在2022年推出了Steam Desk游戏掌机，Steam Desk的Steam OS操作系统是基于已有Linux发行版二次开发的（目前Steam OS属于Arch Linux的衍生版）。  

### 安装Steam
安装steam的方法如下：

在steam下载页面下载steam的deb包，然后使用`apt install PATH_TO_STEAM_DEB`命令安装steam，首次启动steam建议在终端中通过命令steam启动。然后steam可能要求输入提权密码并自动添加i386源并安装steam运行所需i386依赖（该过程中请勿进行任何无关操作，否则可能导致steam崩溃）。然后steam将下载steam 数据包（steam可能不会显示图像化的进度条，如果是通过终端运行的steam可以通过终端输出信息了解steam数据包下载进度）。steam数据包下载并安装完毕后即可登录并使用steam。  

在linux steam客户端中可以下载不少原生支持linux的游戏。即使游戏没有提供原生linux版，大部分Windows游戏也可以通过Valve基于wine针对高性能运行游戏二次开发的Proton运行。Proton对运行windows游戏的支持相当好，就连黑神话悟空都可以通过Proton在linux上完美高性能运行。可以用[ProtonDB](https://www.protondb.com)查询Windows游戏通过Proton在linux上运行的兼容程度。  

### 安装Proton
安装Proton要通过steam，所以需要先安装并登录steam，然后点击steam主页左上角的steam进入设置->“兼容性”菜单，之后选择一个合适的Proton版本将其启用，随后steam将自动下载并安装Proton。  

## 其他游戏运行方案
部分游戏可以通过模拟器运行，如可以通过DosBox运行支持DOS的游戏。  

还有一部分游戏因为其游戏引擎设计的原因其游戏运行时程序和库是固定的，且其运行时、游戏逻辑、美术资源是完全分离的。对于这类游戏引擎制作的游戏，如果有大佬们开发了支持Linux的兼容游戏运行时则可以在Linux游玩该引擎的游戏。如EasyRPG Player即为RPG Maker 2000/2003的兼容游戏运行时。  

**Ruina (废都物语)**  
《废都物语》是一个故事震撼人心的早期日式回合制RPG游戏。其游戏引擎为RPG Maker 2000，故其可以通过EasyRPG Player驱动游戏运行。  

笔者打包了一个Linux和Windows皆可运行的[通用版Ruian](../pool/ruina_universal.tar.xz)，解压后执行对应系统的游戏启动脚本即可运行游戏。  


## 参考资料

\[1\] [repo - steampowered](https://repo.steampowered.com/)  
\[2\] [EasyRPG Player - GAMUX](https://www.linuxgame.cn/easyrpg-player)  
\[3\] [EasyRPG Player](https://easyrpg.org/player/)    

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.7.1 | Date: 2025-10-24
