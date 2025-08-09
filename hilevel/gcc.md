# 安装gcc-15

C23引入了许多新特性，而有的人已经迫不及待地想使用C23进行程序开发了。这时有一个完整支持C23特性的编译器显得尤为重要，而GCC15是第一个完整支持C23特性的GCC版本。但可惜的是GCC15刚好错过了Debian Trixie的软冻结，故GCC15没有收录到常规软件源。但天无绝人之路，我们仍有三种安装GCC15的方法：使用experimental(试制)源、从源码编译或安装第三方编译二进制包，第一种方法比较简单本文不作介绍。  

## 二进制编译安装GCC15

要求：至少8GiB空闲空间
编译编译器要编译器，所以我们需要先安装一个编译器。  
```sh
apt install gcc-14 make gettext build-essential
apt install libgmp-dev libmpfr-dev libmpc-dev libisl-dev
```
然后我们切换到一个合适的目录并下载gcc15源码。  
```sh
wget https://ftp.gnu.org/gnu/gcc/gcc-15.1.0/gcc-15.1.0.tar.xz
```
如果因为本地网络问题下载较慢可以将`https://ftp.gnu.org`部分文本替换为合适的镜像站（如：`https://mirrors.ustc.edu.cn`）  
解压源码包  
```sh
tar -xvf gcc-15.1.0.tar.xz
```
进入源码目录并进行完全清理  
```sh
cd gcc-15.1.0
make distclean
```
设置编译配置  
```sh
./configure \
	--prefix=/opt/gcc-15 \
	--enable-languages=c,c++ \
	--enable-default-pie \
	--enable-shared \
	--disable-bootstrap \
	--disable-werror \
	--enable-threads=posix \
	--enable-checking=release \
	--disable-multilib \
	--with-abi=m64 \
	--enable-nls
```
部分配置解释：  
--prefix=/opt/gcc-15 设置安装路径为/opt/gcc-15，如无此项默认安装路径为/usr/local  
--enable-languages=c,c++ 仅编译用于C和C++的编译器  
--enable-threads=posix 启用符合posix的线程支持（即支持pthread多线程库）  
--enable-checking=release 以发布模式对代码进行一致性检查  
--disable-multilib 禁用multilib（不支持在64位环境中使用-m32选项生成32位程序）  
--enable-shared 允许构建共享库  
--disable-bootstrap 禁用bootstrap编译方式（仅编译一次而非三次）  
--enable-nls 启用本地语言支持（使gcc可以以本地语言输出信息）  

编译及安装  
```sh
make -j$(nproc)
sudo make install-strip
```
注1：`-jN`选项用于设置参与编译的CPU物理线程数，`nproc`用于获取CPU的总物理线程数。  
注2：make子命令`install-strip`指明安装程序与库并剥离调试信息与符号信息（可大幅减少安装占用的磁盘空间）  

收尾工作  
```sh
make distclean
cd ..
rm -rv gcc-15.1.0
```

## 安装第三方编译二进制GCC15

注意：安装第三方编译的gcc具有一定的安全风险！  
这是本人自己编译的gcc15：[gcc-15.1.0_debian-amd64.tar.xz](pool/gcc-15.1.0_debian-amd64~b1.tar.xz)  
安装依赖  
```sh
apt install libc6-dev binutils
```
解压gcc15二进制包  
```sh
sudo tar -xvf /path/to/GCC_15_TARBALL.tar.xz -C /
```
将gcc可执行文件所在目录添加到中PATH（编辑/etc/profile将`/opt/gcc-15/bin:`添加至PATH变量对应字符串的开头）  

## 参考资料

\[1\][Installing GCC](https://gcc.gnu.org/install/)  
\[2\][Installing GCC: Configuration](https://gcc.gnu.org/install/configure.html)  
\[3\][Installing GCC: Final installation](https://gcc.gnu.org/install/finalinstall.html)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.8.2 | Date: 2025-08-09
