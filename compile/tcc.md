# 编译Tiny C Compiler

Tiny C Compiler (tcc)是一个使用C语言编写的超轻量C99编译器，其原作者是大名鼎鼎的Fabrice Bellard<sup>注1</sup>，现tcc由社区接手开发。其具有将C源文件当作脚本运行的独家黑科技，亦提供了libtcc这个运行时动态C代码编译库。  

注1：Fabrice Bellard最著名的作品为FFMPEG和QEMU。  

当前最新的tcc正式版为0.9.27但该版本已经是数年前的版本，该版本存在个别较为严重的bug（如：算术表达式错误的编译结果），且该版本相比最新开发版(0.9.28rc)也少了很多新功能（如：RISC-V 64位架构支持），故如果需要使用tcc，笔者更建议直接下载最新开发版，然后从源码编译安装。tcc编译安装的方法见下文所述。  

tcc支持的C标准：ANSI C, C99 (without inline and complex), 少量C11、C23和C2y特性，部分[GNU C特性](https://www.bellard.org/tcc/tcc-doc.html#GNU-C-extensions)以及独有的[Tiny C特性](https://www.bellard.org/tcc/tcc-doc.html#TinyCC-extensions)。  

## 下载tcc源码
访问[tcc的开发仓库](https://repo.or.cz/w/tinycc.git)，下载shortlog中最新发布的tarball源码包。  

## tcc的编译与安装
通过以下命令解压源码包并进入项目目录。  
```sh
tar -xvf tinycc-COMMITID.tar.gz
cd tinycc-COMMITID
```

然后通过以下命令编译tcc并安装到系统中。  
```sh
./configure
make -j$(nproc)
make test # optional
sudo make install
```
说明：  
`./configure`命令用于检查机器的编译环境并配置生成Makefile文件。  
`make -j$(nproc)`命令用于使用全部处理器线程编译tcc。  
`make test`用于通过测试用例测试编译出来的tcc。（可选）  
`sudo make install`将tcc安装至系统中。  

注：通过`./configure --help`命令可查看编译tcc的配置参数。  

附加说明：在Windows中可通过执行win32文件夹中的build-tcc.bat脚本编译tcc，然后通过`build-tcc.bat -i <dir>`命令将tcc安装至指定目录。  

最后可通过以下命令清理编译目录。  
```sh
make clean
# or
make distclean
```
说明：`clean`将删除编译过程产生的文件，`distclean`则进行更彻底的清理。  

## 参考资料

\[1\] [Tiny C Compiler Reference Documentation](https://www.bellard.org/tcc/tcc-doc.html)（其中内容有所过时）  
\[2\] [Public Git Hosting - tinycc.git/summary](https://repo.or.cz/tinycc.git)  
\[3\] README  
\[4\] win32/tcc-win32.txt  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.7 | Date: 2025-10-24