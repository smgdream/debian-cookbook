# GCC的编译
GCC (GNU Compiler Collection, 旧称 GNU C Compiler)是最著名的编译器之一，其可以编译多种程序设计语言。下面笔者将说明GCC的编译方法。  

## 编译前准备
首先要安装编译所依赖工具和开发库
```sh
apt remove mawk
apt install make gawk flex bison gettext
apt install libgmp-dev libmpfr-dev libmpc-dev libisl-dev
```
然后在[GNU的FTP服务器的gcc目录](https://ftp.gnu.org/gnu/gcc/) 下载gcc源码包。  
注：在部分地区因为地区网络问题在官方FTP服务器下载速率较慢，可通过[GNU的FTP镜像站](https://www.gnu.org/prep/ftp.html)下载gcc（如：`https://mirrors.nju.edu.cn/gnu/gcc/`）。  

之后解压源码包并进入项目目录。  
```sh
tar -xvf gcc-VERSION.tar.xz
cd gcc-VERSION
```

## 编译gcc

编译gcc建议创建一个专门的构建目录（如: `build`）并进入该目录。  
```sh
mkdir build
cd build
```
注：此时所在目录为`gcc-VERSION/build`。  

之后使用以下命令进行编译配置。
```sh
./configure \
	--prefix=/opt/gcc \
	--enable-languages=c,c++ \
	--enable-default-pie \
	--enable-shared \
	--disable-bootstrap \
	--disable-werror \
	--enable-threads=posix \
	--enable-checking=release \
	--disable-multilib \
	--disable-fixincludes \
	--enable-year2038 \
	--enable-nls
```
在该命令中所有参数都是可选的，但笔者添加了这些编译参数也是有原因的，以下是对各个参数的说明，你可以通过该下面的说明判断你是否需要该参数以及是否需要调整参数的值。  

`--prefix=/opt/gcc`  
设置gcc的安装路径为`/opt/gcc`，如无此项默认安装路径为`/usr/local`。  

`--enable-languages=c,c++`  
构建指定程序设计语言的编译器及其运行时库。示例中该参数的值说明了仅构建C和C++的编译器和运行时库。

`--enable-default-pie`
启用位置无关可执行文件功能，让程序可以加载在内存中的任意位置，这可以提高攻击难度，令gcc更加安全。  

`--enable-shared`  
构建共享版的库。如无此项默认构建共享库。  

`--disable-bootstrap`  
禁用三段式编译（又称为三回式编译），顾名思义该编译方式需要编译三次才能得到最终的成品gcc。三段式编译是为了验证编译器的有效性与安全性。但禁用三段式编译能大大减少编译耗时。如果缺少该参数则默认使用三段式编译。  

`--disable-werror`  
禁用编译参数-Werror（如果在编译时使用了参数`-Werror`将会导致将warning当作erorr，一旦遇到warning则直接终止编译）。  

`--enable-threads=posix`  
启用posix多线程支持，如果没有启用posix多线程支持则生成的gcc无法编译posix多线程程序。  

`--enable-checking=release`  
以发布模式对编译结果进行一致性检查，以确保编译出来的程序是正确的。  

`--disable-multilib`  
禁止构建多架构目标库。如无此项则默认构建多架构目标库。（该参数经常使用）  

`--disable-fixincludes`  
禁用头文件修复功能。  

`--enable-year2038`  
启用对2038年之后的时间戳的支持。  


`--enable-nls`  
启用本地语言支持，即可以使用非美式英语（包括中文）输出gcc帮助信息和编译诊断信息。如无此项默启用nls（位于加拿大除外）。  

然后通过以下命令编译gcc。  
```sh
make -j$(nproc)
```
说明：`-j$(nproc)`参数用于使用所有CPU物理线程进行编译。  

**可选:** 如果需要测试编译出来的gcc需要安装`dejagnu`并执行以下命令。  
```sh
make -k check -j$(nproc)
```
注：测试时间较为漫长。  

## 安装gcc
安装gcc可以使用以下命令
```sh
make install-strip
```
说明：安装剥离了调试符号的gcc，而`make install`命令则安装不剥离调试符号的gcc。如果不剥离调试符号则gcc会大好几倍。  

如有打包gcc的需要可以使用如下命令，gcc将被复制到指定的临时安装目录。  
```sh
make DESTDIR=/PATH/TO/TMP_PATH install-strip
```
然后使用所需打包方式的打包命令打包gcc。  

## 清理工作
清理编译目录的命令如下:  
```sh
make clean
# or
make distclean

cd .. && rm -r build # optional
```
说明：`clean`将删除编译过程产生的文件，`distclean`则进行更彻底的清理。最后一行命令（可选）用于删除用于构建的文件夹`build`。    

## 参考资料
\[1\] [GCC, the GNU Compiler Collection](https://gcc.gnu.org/)  
\[2\] [Installing GCC](https://gcc.gnu.org/install/)  
\[3\] [Prerequisites for GCC](https://gcc.gnu.org/install/prerequisites.html)  
\[4\] [Installing GCC: Configuration](https://gcc.gnu.org/install/configure.html)  
\[5\] [Installing GCC: Final installation](https://gcc.gnu.org/install/finalinstall.html)  
\[6\] `./configure --help`  
\[7\] [8.29. GCC-15.2.0 - LFS](https://www.linuxfromscratch.org/lfs/view/stable-systemd/chapter08/gcc.html)  
\[8\] [DejaGnuFAQ - Debian Wiki](https://wiki.debian.org/DejaGnuFAQ)  
\[9\] [16.4 DESTDIR: Support for Staged Installs](https://www.gnu.org/software/make/manual/html_node/DESTDIR.html)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.6 | Date: 2025-10-24