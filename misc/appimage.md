# AppImage食用说明

AppImage是一种用于Linux的便携式单文件可执行文件的打包。换言之，AppImage就是一个单文件可执行文件，他将一个应用运行所需的几乎一切文件（程序、资源、依赖库等）打包到了一个可执行文件中，无须安装即可直接运行。其中程序依赖的库已包含在包体中所以系统中库是否安装、版本几何都不妨碍其运行。  

注：如果以上说明对于你来说仍难以理解，你可以简单粗暴地将appimage当作一类单文件绿色软件。   

如今不少软件都会提供AppImage分发，如果一个软件提供了AppImage分发基本都会在下载页中列出。通常我们获取到的AppImage文件的后缀名为`AppImage`。  

对于Debian Trixie在运行AppImage前还要确认是否安装了`libfuse2t64`库，如果尚未安装则要通过包管理器将其安装。否则AppImage可能无法直接运行只能解压运行。  

运行AppImage首先要确认其是否有执行权限，如果没有则需要先通过以下命令赋予其权限。  
```sh
chmod +x <application>.AppImage
```
然后在文件管理器中双击运行或通过终端直接运行。 

当然AppImage并非一个简单的归档而是一个压缩归档。当我们执行AppImage时，他首先要进行自解压并挂载到一个临时目录，而这个解压挂载的过程必然会花费些许时间，对于用户来说其会令程序的启动时间变长。如果我们想要避免启动时解压挂载的耗时则要将AppImage解压到一个位置，之后直接运行解压目录中应用程序的AppRun脚本即可启动程序。通常我们使用以下命令来解压AppImage。  
```sh
./<application>.AppImage --appimage-extract
```
然后在前的工作目录中将会出现一个`squashfs-root`目录，该目录即为AppImage的解压目录。`squashfs-root`目录中存在一个`AppRun`脚本/程序，该脚本即为主程序的启动脚本。之后我们可以将`squashfs-root`重命名为一个合适的名字然后移动到一个合适的位置并为其编写一个desktop启动器配置指向AppRun。  

AppImage包除了提供程序本身的参数以外基本上还会提供一套于程序本身无关的appimage选项参数。通过`--appimage-help`参数即可查看AppImage提供的选项参数。  

示例  
```sh
./<application>.AppImage --appimage-help
```

## 其他AppImage使用小技巧
如果我们想要直接为AppImage创建一个desktop启动器，我们手动编写一个desktop配置自然是可以的。但这有点麻烦，我们可以使用[AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher)来方便的为AppImage创建desktop启动器。  

通常AppImage包中会包含一个程序的图标文件但在文件管理器的并不会显示该文件缩略图，因为系统没有预装AppImage的缩略图生成器。生成AppImage的缩略图可以使用[xapp-appimage-thumbnailer](http://packages.linuxmint.com/pool/main/x/xapp-thumbnailers/)。  

在Debian的官方源中不存在xapp-appimage-thumbnailer，需自行前往其[deb包池](http://packages.linuxmint.com/pool/main/x/xapp-thumbnailers/)中下载版本号一致的xapp-appimage-thumbnailer deb包和xapp-thumbnailers-common deb包，然后通过包管理器安装这两个包后文件管理器即可渲染出AppImage文件缩略图<sup>注1</sup>。  

注1：在Nautilus中Appimage的缩略图显示存在一点小问题，图标中的透明部分会显示为灰白色的棋盘格而非文件管理器的背景色，但缩略图缓存的图片并没有棋盘格，这说明棋盘格是Nautilus自己加上去的。  

## 运行AppImage可能会遇到的问题

部分使用了Web技术制作UI的程序的AppImage分发版可能无法直接运行（常见于国产闭源软件的AppImage构建）。对于这类AppImage程序，若要令其正确运行需添加运行参数`--no-sandbox`。如: `./xxmusic.AppImage --no-sandbox`。如果要解压运行此类AppImage，则要为其AppRun脚本中运行主程序的命令加上参数`--no-sandbox`。 

## 附加信息
根据AppImage的官方文档AppImage分为两种类型，分别称为Type 1和Type 2，如今笔者遇到的AppImage都是Type 2。Type 2可以通过--appimage-extract参数来解压，而Type 1则需用mount来挂载。网上关于Type 1的资料少得可怜，官方文档亦是如此。根据官方文档的零星几处描述，推测其是早期的AppImage打包形式，其不包含AppImage运行时，可能就是一个不可执行的归档，需要通过专门的加载器运行。  

## 参考资料
\[1\] [AppImage Linux apps that run anywhere](https://appimage.org/)  
\[2\] [Welcome to the AppImage documention](https://docs.appimage.org/)

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.2u | Date: 2026-10-16