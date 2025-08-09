# Linux游戏
当今在linux中也可以体验到不少游戏，但和windows相比还是差了点。  

## 原生游戏

**minecraft**  
说起原生支持linux的游戏，minecraft是绕不过的话题。minecraft使用java编写，所以只要目标平台能跑jvm它就能跑minecraft。在debian中运行mc的方法如下：  
首先需要安装jre（java runtime environment）。  
```sh
apt install openjdk-21-jre
```
然后下载一个mc启动器（如[hmcl](https://hmcl.huangyuhui.net/)）。  
之后即可通过java -jar hmcl.jar命令启动启动器，然后在启动器中下载mc并启动。  

**其他游戏**  
现在随着steam desk的发展和linux桌面用户的增长以及godot游戏开发者的增加。越来越多游戏加入了原生支持linux的队伍，如：ender lilies、half-life、BUCKSHOT ROULETTE等游戏。  

## Wine游戏

wine不单是为了运行windows应用，高性能地运行windows的游戏也是wine更新至今的一大目标。关于wine的安装方法见[Wine](wine.md)。因为wine不是模拟器所以通过wine运行游戏性能损耗也较小，有的游戏甚至还比在windows上运行的帧率更高。  

通常使用命令`wine GAME_PROGRAM`即可通过Wine运行Windows游戏。在部分机器上linux可能无法在wine运行游戏时将显卡设为高性能显卡，此时可以可以通过在程序运行命令前添加变量`__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only`即可指定使用nv独显渲染游戏（命令示例：`__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only wine GAME_PROGRAM`），在其他linux原生应用程序运行命令前添加这些变量也可以将程序的渲染显卡指定成nv独显。  

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

**GRIS**  
wine版本要求：>= 6.0  
运行方法：  
```sh
wine GRIS.exe
```
bug: 从主界面中退出游戏时可能会GC错误无法结束关闭界面（强制结束游戏进程即可，不影响游戏存档）。
<br>

**NieR:Automata（尼尔：机械纪元）**  
wine版本要求：>= 9.0  
运行方法：  
```sh
wine NieRAutomata.exe
```
<br>

**Ruina（废都物语）**  
wine版本要求：>= 8.0  
运行方法：  
```sh
wine Ruina.exe
```
<br>

**Monument Valley I（纪念碑谷  I）**  
wine版本要求：>= 6.0  
运行方法：  
```sh
wine 'Monument Valley.exe'
```
第一次执行游戏时如果无法进入游戏游玩界面则强制结束游戏进程并再次启动。  
<br>

**Monument Valley II（纪念碑谷  II）**  
wine版本要求：>= 7.0  
运行方法：  
```sh
wine 'Monument Valley 2.exe'
```
第一次执行游戏时如果无法进入游戏游玩界面则强制结束游戏进程并再次启动。  
<br>

**Monument Valley III（纪念碑谷  III）**  
wine版本要求：>= 10.0  
运行方法：  
```sh
wine 'Monument Valley 3exe'
```
<br>

**ENDER LILIES（终焉的莉莉丝）**  
wine版本要求：>= 9.0  
运行方法：  
```sh
wine EnderLilies.exe
```
第一次执行游戏时可能会弹出运行库安装界面，安装完运行库后即可正常运行游戏。  
bug：在部分场景中游戏角色可能因为wine渲染bug而缺少角色贴图，角色将被渲染成橙色剪影。  
<br>

**ENDER MAGNOLIA（终焉的玛格诺利亚）**  
wine版本要求：>= 10.0  
运行方法：  
首先通过以下命令安装运行库。  
```sh
wine Engine/Extras/Redist/en-us/UEPrereqSetup_x64.exe
```
然后通过以下命令运行游戏。  
```sh
wine EnderMagnolia/Binaries/Win64/EnderMagnoliaSteam-Win64-Shipping.exe
```
注1：请勿通过`EnderMagnolia.exe`启动游戏，否则游戏运行将出现问题甚至无法运行。  
注2：在部分机器上也许通过以上的方法仍无法运行游戏，而如果机器有NVIDIA独显则可以尝试通过以下命令运行游戏。
```sh
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only wine EnderMagnolia/Binaries/Win64/EnderMagnoliaSteam-Win64-Shipping.exe
```
<br>

**Journey（风之旅人）**  
wine版本要求：>= 9.0  
运行方法：  
通过以下命令启动wine explorer：  
```sh
wine explorer
```
然后在explorer中打开游戏目录并双击运行游戏。  



## steam

steam早已原生支持linux，而Valve在2022年推出了steam desk，steam desk的Steam OS操作系统是基于linux发行版二次开发的（目前Steam OS属于Arch Linux的衍生版）。  

**安装steam**
安装steam的方法如下：  
在steam下载页面下载steam deb包，然后使用`apt install PATH_TO_STEAM_DEB`命令安装steam。  

在linux steam客户端中可以下载不少原生支持linux的游戏。即使游戏没有提供原生linux版，大部分Windows游戏也可以通过Valve基于wine针对高性能运行游戏二次开发的Proton运行。Proton对运行windows游戏的支持相当好，就连黑神话悟空都可以通过Proton在linux上完美高性能运行。可以用[ProtonDB](https://www.protondb.com)查询Windows游戏通过Proton在linux上运行的兼容程度。  

**安装Proton**  
安装Proton要通过steam，所以需要先安装并登录steam，然后点击steam主页左上角的steam进入设置->“兼容性”菜单，之后选择一个合适的Proton版本将其启用。  


## 参考资料

\[1\] [repo - steampowered](https://repo.steampowered.com/)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.4 | Date: 2025-08-04
