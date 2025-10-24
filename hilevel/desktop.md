# Desktop启动器

桌面项(Desktop Entry)文件是程序的启动器配置文件，其作用与Android的应用启动项或windows中的快捷方式类似。  

桌面项文件的后缀名为`desktop`，desktop文件的存放目录如下：  
系统：`/usr/share/applications/`
用户：`~/.local/share/applications`

## desktop配置文件内容格式说明

desktop文件以`[Desktop Entry]`开头，注释格式为`# comment`。除此之外其他内容均为条目键值对，条目键值对的格式为`KEY=VALUE`，其中常用的条目键值对如下：  
| 键 | 可选性 | 描述 | 有效值及其描述 |
| --- | --- | --- | --- |
| `Type` | 必选 | 用于说明该桌面入口文件启动的对象类型 | `Application`表示启动的是可执行文件 ，`Link`表示启动的是网络链接 |
| `Name` | 必选 | 展示出来的应用名称 | 如：Firefox |
| `Icon` | 可选 | 设置启动项的图标，支持的图标文件类型为png和svg | 图标文件的路径，如：/opt/pxtrix/pxtrix.png |
| `Exec` | 可选 | 所要执行的程序路径 | 如：`/opt/pxtrix/pxtrix.run` |
| `URL` | 可选 | 当`Type`为`Link`时其值为所需访问链接的URL | 如：`https://www.kernel.org` |
| `Terminal` | 可选 | 程序是否在终端中运行 | `true`或（默认）`false` |
| `Path` | 可选 | 设置程序运行时的工作目录 | 如：`/tmp` |
| `NoDisplay` | 可选 | 用于设置是否在应用菜单中显示应用启动器，但不影响在文件打开方式中选择使用，所以可以用于设置文件打开方式项（并在应用菜单中不显示） | `true`或`false` |
| `MimeType` | 可选 | 程序所支持打开的文件的MIME类型（如果没有此项则假设支持所有文件类型） | 如：`application/x-krita;image/openraster;application/x-krita-paintoppreset;`，各MIME类型需以`;`结尾 |
| `PrefersNonDefaultGPU` | 可选 | 当值为`true`时则偏向于默认使用“更高性能的非默认GPU”（即独立显卡）运行程序 | `true`或（默认）`false` |
| `X-KDE-RunOnDiscreteGpu` | 可选（非标准的条目键值对） | 使用独立GPU运行程序（可配合`PrefersNonDefaultGPU`使用） | `true`或（默认）`false` |
| `Version` | 可选 | Desktop Entry规范版本 | 如：`1.5` |

欲想了解更多相关信息见：[Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest-single/)和[Desktop entries - ArchWiki](https://wiki.archlinux.org/title/Desktop_entries)  

## desktop文件示例

**Krita desktop文件**  
```
[Desktop Entry]
Name=Krita
Exec=/opt/krita/AppRun
Icon=/opt/krita/krita.png
GenericName=Digital Painting
MimeType=application/x-krita;image/openraster;application/x-krita-paintoppreset;
Comment=Digital Painting
Type=Application
Categories=Qt;KDE;Graphics;2DGraphics;RasterGraphics;
X-KDE-NativeMimeType=application/x-krita
X-KDE-ExtraNativeMimeTypes=
StartupNotify=true
X-Krita-Version=28
StartupWMClass=krita
# Always be the preferred handler for .kra files
InitialPreference=99
```

**一个desktop文件模板**  
```
[Desktop Entry]
Type=Application
Name=NAME
Exec=PROGRAM
Icon=ICON_FILE

# FOR POWERFUL GPU
#PrefersNonDefaultGPU=true
#X-KDE-RunOnDiscreteGpu=true
```

## 参考资料

\[1\] [Desktop entries - ArchWiki](https://wiki.archlinux.org/title/Desktop_entries)  
\[2\] [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest-single/)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.6 | Date: 2025-10-10
