# 自定义文件缩略图生成
安装配置好了Debian Trixie结果发现除了图片其他文件都无法加载出文件缩略图，但别人的系统却没有问题这是为何？  

真相大概率是你把totem卸载了。因为totem还个隐藏功能，即大部分多媒体文件类型的缩略图都是由它来生成的。所以将它删了以后大部分文件都生成不了缩略图。所以为了生成缩略图是必须得保留totem吗？实际上gnome的缩略图生成机制没那么糟糕，缩略图生成并非依赖于totem，而是通过读取缩略图生成配置文件进而去调用缩略图生成器生成缩略图。  

## thumbnailer文件
国内外关于该文件都比较稀缺，本人为了找到相对完整的的资料花了不少功夫。  
thumbnailer文件类似于desktop文件，desktop文件是应用程序的启动器，而thumbnailer文件是缩略图生成的启动器。当打开图形文件管理器时，gnome将确认打开目录中各文件对应文件类型是否有缩略图生成配置文件支持生成该文件类型的缩略图，如果有则调用对应程序生成缩略图（如果存在支持对应文件类型的缩略图生成配置但无法生成生成正常的缩略图\[如：没有封面的flac文件\]，则生成一个1x1的垃圾缩略图，文件管理器将忽略该垃圾缩略图并显示该文件类型的原始图标），并将缩略图文件缓存至`~/.cache/thumbnails`文件夹中，如果没有则不予理会。当以后再次打开文件夹，如果对应文件已存在缩略图缓存且文件未发生改变则直接使用，不再重新生成缩略图，否则将尝试再次生成缩略图。  

thumbnailer文件通常存放在`/usr/share/thumbnailers`目录中，该文件的常见格式如下：  
```thumbnailer
[Thumbnailer Entry]

TryExec=PROGRAM
Exec=PROGRAM -i %i -o %o -s %s args...
# or Exec=PROGRAM -s %s %u %o args...

MimeType=video/mp4;audio/flac;application/pdf;

# 以上示例仅供参考
```
`[Thumbnailer Entry]`: 固定标识，需置于首行。  
Exec（必选）：用于执行缩略图生成器，它支持几个在调用前被替换的参数：  
`%i`: 要生成缩略图的源文件路径  
`%u`: 要生成缩略图的源文件URI  
`%o`: 生成的缩略图的路径  
`%s`: 缩略图的最大大小（边长：像素）  
`%%`: 百分号字符字面值  
TryExec（可选）：用于确定程序确实安装在系统里面。  
PROGRAM: 程序的绝对路径或$PATH中的程序。  
MimeType（必选）：可用缩略图生成器生成缩略图的文件的Mime类型（各Mime类型以分号分隔，最后以分号结尾）  

注：MIME相关的资料可见[Complete List of All MIME Types](https://mime.wcode.net/), [Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml) or [Check MIME types from files](https://mimetype.io/)  

## 基于ffmpegthumbnailer的缩略图生成配置
假如我们确实不用totem，也不想为了一个缩略图而保留totem。那我们可以卸载totem，然后使用ffmpegthumbnailer来生成缩略图。  

安装ffmpegthumbnailer  
```sh
apt install ffmpegthumbnailer
```

然后我们发现只有视频文件缩略图生成是正常的，音频文件无法生成缩略图。这是因为ffmpegthumbnailer对应的thumbnailer文件中的MimeType字段并没有音频文件相关的Mime类型。实际上ffmpegthumbnailer支持生成音频缩略图，所以只要在MimeType字段中添加音频文件相关的Mine类型即可。  

原本的ffmpegthumbnailer对应的thumbnailer文件中`Exec`对应的执行语句有一个`-f`参数，该参数用于中生成视频缩略图时加上胶片打孔的效果，这会增加生成缩略图的耗时并占用更多资源，而且大多数情况下该参数也是没意义的（其中的缘由请看**题外话**段落），所以我们可以将该参数删除。（关于ffmpegthumbnailer.thumbnailer的原始内容见**附加信息**段落）  

忘记说了ffmpegthumbnailer对应的thumbnailer文件路径为`/usr/share/thumbnailers/ffmpegthumbnailer.thumbnailer`  
以下是一个修改后的ffmpegthumbnailer.thumbnailer示例:  
```thumbnailer
[Thumbnailer Entry]

TryExec=ffmpegthumbnailer
Exec=ffmpegthumbnailer -i %i -o %o -s %s
MimeType=video/mp4;video/mpeg;video/ogg;video/vnd.avi;video/webm;video/x-flv;video/x-mjpeg;video/x-ms-wmv;video/x-ogm+ogg;video/x-theora+ogg;application/vnd.ms-asf;application/x-matroska;video/x-matroska;application/ogg;audio/flac;audio/mpeg;audio/ogg;
```
说明：删除了Exec字段执行语句参数中的`-f`。删除了部分不常用文件对应的MIME类型。添加了常用音频文件格式对应的MIME类型。  

建议配置完后通过`rm -rv ~/.cache/thumbnails`清空缩略图缓存，如果缩略图缓存占用空间较大也可以通过此命令清理。  

等等，那图片及其他文件的缩略图生成去哪了？这个你不用担心，常用图片类型及部分其他文件类型的缩略图生成由由其他一些系统预装的没图形界面的软件负责，正常情况下也不会卸载这些软件。但是部分文件类型系统没有预装对应的缩略图生成器，这些文件的缩略图生成器需要用户自行安装。（通过命令`apt list *thumbnailer*`可以查看软件源中存在的缩略图生成器）  

## 题外话
也许你照我上面的示例配置好了ffmpegthumbnailer，但你突然发现为什么视频文件仍然有胶片打孔样式。但当你打开缩略图缓存文件一看发现图片并没有打孔样式。自此真相大白，胶片样式是图形文件管理器自己叠上去的。这也就是为何我说大部分情况下`-f`参数没有意义的原因。  

## 附加信息

以下是ffmpegthumbnailer对应的thumbnailer文件的原始内容  
```thumbnailer
[Thumbnailer Entry]
TryExec=ffmpegthumbnailer
Exec=ffmpegthumbnailer -i %i -o %o -s %s -f
MimeType=video/3gpp;video/3gpp2;video/annodex;video/dv;video/isivideo;video/mj2;video/mp2t;video/mp4;video/mpeg;video/ogg;video/quicktime;video/vnd.avi;video/vnd.mpegurl;video/vnd.radgamettools.bink;video/vnd.radgamettools.smacker;video/vnd.rn-realvideo;video/vnd.vivo;video/vnd.youtube.yt;video/wavelet;video/webm;video/x-anim;video/x-flic;video/x-flv;video/x-javafx;video/x-matroska;video/x-matroska-3d;video/x-mjpeg;video/x-mng;video/x-ms-wmv;video/x-nsv;video/x-ogm+ogg;video/x-sgi-movie;video/x-theora+ogg;application/mxf;application/vnd.ms-asf;application/vnd.rn-realmedia;application/x-matroska;application/ogg;
```

以下是totem对应的thumbnailer文件的原始内容  
```thumbnailer
[Thumbnailer Entry]
TryExec=/usr/bin/totem-video-thumbnailer
Exec=/usr/bin/totem-video-thumbnailer -s %s %u %o
MimeType=application/mxf;application/ram;application/sdp;application/vnd.apple.mpegurl;application/vnd.ms-asf;application/vnd.ms-wpl;application/vnd.rn-realmedia;application/vnd.rn-realmedia-vbr;application/x-extension-m4a;application/x-extension-mp4;application/x-flash-video;application/x-matroska;application/x-netshow-channel;application/x-quicktimeplayer;application/x-shorten;image/vnd.rn-realpix;image/x-pict;misc/ultravox;text/x-google-video-pointer;video/3gp;video/3gpp;video/3gpp2;video/dv;video/divx;video/fli;video/flv;video/mp2t;video/mp4;video/mp4v-es;video/mpeg;video/mpeg-system;video/msvideo;video/ogg;video/quicktime;video/vivo;video/vnd.avi;video/vnd.divx;video/vnd.mpegurl;video/vnd.rn-realvideo;video/vnd.vivo;video/webm;video/x-anim;video/x-avi;video/x-flc;video/x-fli;video/x-flic;video/x-flv;video/x-m4v;video/x-matroska;video/x-mjpeg;video/x-mpeg;video/x-mpeg2;video/x-ms-asf;video/x-ms-asf-plugin;video/x-ms-asx;video/x-msvideo;video/x-ms-wm;video/x-ms-wmv;video/x-ms-wmx;video/x-ms-wvx;video/x-nsv;video/x-ogm+ogg;video/x-theora;video/x-theora+ogg;video/x-totem-stream;audio/x-pn-realaudio;audio/3gpp;audio/3gpp2;audio/aac;audio/ac3;audio/AMR;audio/AMR-WB;audio/basic;audio/dv;audio/eac3;audio/flac;audio/m4a;audio/midi;audio/mp1;audio/mp2;audio/mp3;audio/mp4;audio/mpeg;audio/mpg;audio/ogg;audio/opus;audio/prs.sid;audio/scpls;audio/vnd.rn-realaudio;audio/wav;audio/webm;audio/x-aac;audio/x-aiff;audio/x-ape;audio/x-flac;audio/x-gsm;audio/x-it;audio/x-m4a;audio/x-m4b;audio/x-matroska;audio/x-mod;audio/x-mp1;audio/x-mp2;audio/x-mp3;audio/x-mpg;audio/x-mpeg;audio/x-ms-asf;audio/x-ms-asx;audio/x-ms-wax;audio/x-ms-wma;audio/x-musepack;audio/x-opus+ogg;audio/x-pn-aiff;audio/x-pn-au;audio/x-pn-wav;audio/x-pn-windows-acm;audio/x-realaudio;audio/x-real-audio;audio/x-s3m;audio/x-sbc;audio/x-shorten;audio/x-speex;audio/x-stm;audio/x-tta;audio/x-wav;audio/x-wavpack;audio/x-vorbis;audio/x-vorbis+ogg;audio/x-xm;application/x-flac;
```
<br>

# 参考资料
\[1\] [libgnome-desktop/gnome-desktop-thumbnail.c · master · undefined · GitLab](https://gitlab.gnome.org/GNOME/gnome-desktop/-/blob/master/libgnome-desktop/gnome-desktop-thumbnail.c)  
\[2\] [How to create custom thumbnailers for Nautilus, Nemo, and Caja?](https://askubuntu.com/questions/1368910/how-to-create-custom-thumbnailers-for-nautilus-nemo-and-caja)  
\[3\] [使用 ffmpegthumbnailer替代GNOME下totem快速生成缩略图](https://www.bilibili.com/opus/753857861195399217)  
\[4\] [desktop（桌面入口）文件规范](https://wiki.deepin.org/zh/03_%E6%8A%80%E6%9C%AF%E8%A7%84%E8%8C%83/02_XDG%E8%A7%84%E8%8C%83/desktop%E6%96%87%E4%BB%B6%E8%A7%84%E8%8C%83)  
\[5\] [Desktop file](https://develop.kde.org/docs/features/desktop-file/)  
\[6\] [常见 MIME 类型列表](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Guides/MIME_types/Common_types)  
\[7\] [Complete List of All MIME Types](https://mime.wcode.net/)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.9 | Date: 2025-06-28
