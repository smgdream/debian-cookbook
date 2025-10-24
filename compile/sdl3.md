# SDL3的编译

SDL3是一个简单的2D多媒体开发库，它提供了对HDR、vulkan、HiDPI、wayland等现代多媒体特性的支持。其编译方式如下文所述。  

## 编译SDL3

首先，在SDL3的[Github发布页](https://github.com/libsdl-org/SDL/releases)下载稳定版的SDL3源码。  

然后通过以下命令解压源码包并进入项目目录。  
```sh
tar -xvf SDL3-VERSION.tar.gz
cd SDL3-VERSION
```

之后安装编译SDL3所依赖的工具和开发库。
```sh
apt install make pkg-config cmake 
apt install libasound2-dev libpulse-dev \
libaudio-dev libjack-dev libsndio-dev libx11-dev libxext-dev \
libxrandr-dev libxcursor-dev libxfixes-dev libxi-dev libxss-dev \
libxkbcommon-dev libdrm-dev libgbm-dev libgl1-mesa-dev libgles2-mesa-dev \
libegl1-mesa-dev libdbus-1-dev libibus-1.0-dev libudev-dev \
libpipewire-0.3-dev libwayland-dev libdecor-0-dev liburing-dev
```
注：以上依赖为在Debian中构建SDL3所需依赖。  

然后使用以下命令构建SDL3并将动态库安装至指定目录。  
```sh
cmake -S . -B build
cmake --build build -j$(nproc)
cmake --install build --prefix DIRECTORY
```
说明：  
`cmake -S . -B build -DCMAKE_BUILD_TYPE=Release`  
检查机器的编译环境，设置构建目录为build，指定构建模式为Release，然后生成编译脚本。  

注1：如果不设置`-DCMAKE_BUILD_TYPE=Release`参数则默认的构建模式为RelWithDebInfo，该模式将保留部分调试信息，构建出来的文件也自然会大一些。  
注2：在Windows中使用MinGW进行编译，如果没有安装ninja则需要添加参数`-G "MinGW Makefiles"`将生成器指定为MinGW Makefiles。  

`cmake --build build -j$(nproc)`  
编译SDL3将构建目录设置为build并使用所有的CPU线程进行编译。  
`cmake --install build --prefix DIRECTORY`  
从构建目录bulid中将SDL3安装到指定目录中。后续即可使用该目录中SDL3头文件和动态库。  

如需进行清理工作删除build目录即可。  

## 参考资料
\[1\] docs/README-linux.md  
\[2\] docs/README-cmake.md  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.6.2 | Date: 2025-10-24