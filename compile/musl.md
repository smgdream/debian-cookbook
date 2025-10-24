# musl libc的编译

musl libc是一个轻量级的适用于Linux内核的C标准库实现，其向上提供了ISO C标准库、POSIX等一系列常用C接口。其编译方式见下文的说明。  

## 编译musl

先在[musl官方git仓库](https://git.musl-libc.org/cgit/musl)中下载musl。  

然后使用以下命令解压源码包并进入项目目录。  
```sh
tar -xvf musl-VERSION.tar.gz
cd musl-VERSION
```

然后使用以下命令编译并安装musl
```sh
./configure
make -j$(nproc)
make install
```
说明：  
`./configure`用于检测编译环境并根据（可能提供的）编译参数生成相应的Makefile。  
注：可通过`./configure --help`命令查看可以使用的编译配置参数。  
`make -j$(nproc)`用于使用所有CPU线程构建musl。  
`make install`用于安装musl。 

### 在宿主机上为目标机编译安装musl
仅第一第三条命令需要更改。
对于目标机安装目录为其根目录，故需要添加`--prefix`参数并将其值指定为`/`。  
```sh
./configure --prefix=/
```

因为要在宿主机上为挂载的目标机安装musl，所以在安装时需通过`DESTDIR`参数指定目标机的根目录挂载点。  
```sh
make DESTDIR=/TARGET/ROOT/MNT/POINT install
```
说明：`DESTDIR`参数实际用于将软件复制到临时位置而不是`--perfix`（或默认）指定的期望安装目录。虽然在该例子中虽然在宿主机上软件复制到的位置是通过`DESTDIR`设置的临时位置，但在目标机脱离宿主机独立运行后，则软件的安装位置即成为了`--perfix`参数所指定的期望安装目录`/`。  

## 清理工作
清理编译目录的命令如下:  
```sh
make clean
# or
make distclean
```
说明：`clean`将删除编译过程产生的文件，`distclean`则进行更彻底的清理。  

## 参考资料
\[1\] [musl libc](https://musl.libc.org/)  
\[2\] [16.4 DESTDIR: Support for Staged Installs](https://www.gnu.org/software/make/manual/html_node/DESTDIR.html)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.7 | Date: 2025-10-24


