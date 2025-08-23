# 基本开发环境配置
以下内容仅供参考，请根据实际需要选择性配置。  

## 常用构建系统
绝大部分软件的构建都需要构建系统，所以我们必然要安装常用的构建系统。命令如下：  
```sh
apt install make cmake meson
```
说明：  
make：GNU软件及微小型项目常用构建系统  
cmake：大型项目常用构建系统  
meson：python编写的较常用的构建系统  

## GNU C/C++ 编译环境
debian中的默认awk为mawk，而GNU软件及其他部分软件的编译依赖于gawk的特性所以需要用gawk替换掉mawk。命令如下  
```sh
apt remove mawk
apt install gawk
```

安装gcc编译器。命令如下：  
```sh
apt install gcc-14
```
注：Debian trixie没有提供gcc15，如果需要使用gcc15可参考[GCC-15](hilevel/gcc.md)。  

安装GNU程序编译可能依赖的程序，命令如下：  
```sh
apt install flex bison gettext
```
说明：  
flex：词法分析器  
bison： 语法分析器  
gettext：GNU国际化处理套件  

安装程序开发时可以使用的实用工具， 命令如下：  
```sh
apt install cppcheck cloc
```
说明：  
cppcheck：可用于C和C++的静态代码分析工具  
cloc：代码行统计工具  

常用GNU数学开发库安装，命令如下：  
```sh
apt install libgmp-dev libmpfr-dev libmpc-dev libisl-dev
```
说明：  
libgmp-dev：GNU多精度大数计算开发库  
libmpfr-dev：GNU多精度浮点计算开发库  
libmpc-dev：GNU多精度复数浮点计算开发库  
libisl-dev：整数点集运算开发库  

安装其他常用开发库，命令如下：  
```sh
apt install libssl-dev libncurses-dev libelf-dev
```
说明：  
libssl-dev：常用的著名的密码学开发库  
libncurses-dev：TUI界面程序开发框架  
libelf-dev：ELF文件处理开发库  

## 安装常用脚本语言解释器
命令如下：  
```sh
apt install python3 perl
```
说明：  
python3：python脚本解释器  
perl：perl脚本解释器  

## git版本管理
安装git进行项目版本管理，命令如下：  
```sh
apt install git
```

## Linux内核头文件
部分与内核有关的软件的编译或安装依赖于Linux内核头文件，Linux内核相关的程序开发也需要该组件，安装命令如下：  
```sh
apt install linux-headers-$(uname -r | sed 's/.*-//g')
```
注：`uname -r | sed 's/.*-//g'`子命令用于获取合适格式的机器架构。  

## 安装deb包开发工具
该软件用于将现有软件打包成deb包及用于修改deb包。安装命令如下：  
```sh
apt install dpkg-dev
```
## Linux图形服务器开发库
安装命令：  
```sh
apt install libx11-dev libwayland-dev
```
说明：  
libx11-dev：x11图形服务器开发库  
libwayland-dev：wayland图形服务器开发库  

## 底层图形程序开发
关于openGL和vulkan图形程序编程接口开发库的安装。  

### 安装openGL
安装命令：  
```sh
apt install libgl-dev
```

### 安装vulkan
**apt安装**  

安装命令：  
```sh
apt install vulkan-dev
```

**离线安装**  

从vulkan sdk下载页的Linux下载项的SDK Tarball选项卡下载最新版的vulkan sdk xz压缩档。然后解压到一个合适的目录（如：/opt，通过命令`tar -xvf vulkan-sdk-linux-*.tar.xz -C /opt`）。之后将解压出来的以版本号命名的目录重命名成一个合适的名字（如：vulkansdk，通过命令`mv x.x.xxx.x vulkansdk`）。然后授予目录中的`setup-env.sh`脚本以可执行权限（通过命令 `chmod +x setup-env.sh`）。最后通过命令`ln -s /VULKAN_SDK_DIR/setup-env.sh /etc/profile.d/vulkansdk.sh`创建软链接以设置vulkan sdk的环境变量（彻底登出用户或重启计算机即可生效）。  

## dotnet开发环境
想在Linux在进行C#程序开发需要安装dotnet开发环境，安装方法如下（以net8举例）：  

**apt安装**  
下载并添加包签名密钥并添加包存储库。  
```sh
wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
```
安装dotnet8  
```sh
apt update
apt install dotnet-sdk-8.0 -y
```

**离线安装**  
从[dotnet官网的下载页](https://dotnet.microsoft.com/en-us/download)的对应版本[下载页](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)下载适用于linux的对应机器架构的最新的Binary压缩包。然后在一个合适的位置（如：/opt）创建一个用于安装dotnet的目录（如：dotnet，通过命令`mkdir /opt/dotnet`）。之后将通过命令`tar -xvf dotnet-sdk-*.tar.gz -C DOTNET_DIR`将压缩包解压到对应目录。然后在dotnet安装目录中创建一个名为`dotnet-path.sh`的shell脚本文件并在其中写入以下内容：  
```sh
#! /bin/sh

export PATH="$PATH:DOTNET_DIR:$DOTNET_DIR/tools"
```
注：请将DOTNET_DIR替换为dotnet安装目录的绝对路径。  
然后授予目录中的`dotnet-path.sh`脚本以可执行权限（通过命令 `chmod +x dotnet-path.sh`）。最后通过命令`ln -s /DOTNET_DIR/dotnet-path.sh/etc/profile.d/dotnet.sh`创建软链接以设置dotnet的PATH变量（彻底登出用户或重启计算机即可生效）。  

## java开发环境
通过以下命令安装java sdk：  
```sh
apt install openjdk-21-jdk openjfx # can update to openjdk25 after java25 release
```

## Rust开发环境
**在线安装**  
通过以下命令安装rust：  
```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

**离线安装**  
从[Other Rust Installation Methods页面](https://forge.rust-lang.org/infra/other-installation-methods.html)下载适用于linux系统的对应机器架构的最新稳定版独立安装包。然后使用`tar -xvf rust-*.tar.xz`解压安装包，然后进入安装包目录以root用户执行`./install.sh`安装脚本即可完成rust开发环境的安装（可通过设置安装脚本的参数来调整rust的安装，有关参数的详细信息可通过`./install.sh --help`查看）。  

## 安卓设备调试环境
通过以下命令安装调试安卓设备所需工具：  
```sh
apt install adb fastboot
```

## 安装QEMU
通过以下命令安装QEMU虚拟机：
```sh
apt install qemu-system
```

## 参考资料
\[1\] [Install the .NET SDK or the .NET Runtime on Debian](https://learn.microsoft.com/en-us/dotnet/core/install/linux-debian?tabs=dotnet8)  
\[2\] [Install .NET on Linux by using an install script or by extracting binaries](https://learn.microsoft.com/en-us/dotnet/core/install/linux-scripted-manual#manual-install)  
\[3\] [Getting started - Rust Programming Language](https://www.rust-lang.org/learn/get-started)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.7.3 | Date: 2025-08-21
