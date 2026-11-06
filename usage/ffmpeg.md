# FFmpeg使用简述（简化版）

FFmpeg (Fast Forward mpeg)是一个先进的开源多媒体框架，其可用于多媒体（视频、音频、图像）的复用、解复用、编解码和处理。FFmpeg包含一个核心工具ffmpeg以及两个附加工具ffplay和ffprobe。但FFmpeg不单是个命令行多媒体处理工具，它同时还是个多媒体处理开发库，这部分内容属于软件开发范畴，本文不作介绍。  

<!-- 是否要分离如果分离那个先那个后？ -->

## 多媒体基础知识
### 容器
对于部分先进的多媒体文件格式都可以将其看作容器，其内部可容纳各种各样的以编码的多媒体数据，如mkv(Matroska Video)格式可以容纳多种编码的音视频流、字幕及额外的附件。而mp4格式则只能容纳寥寥几种音视频流。

### 流
流是一段连续的多媒体数据，比如一段音频流，一段视频流。
<!-- 图像、视频、音频结构的介绍？ -->

### 编码
流中的多媒体数据是按照一定的规则存放的，这种规则即为编码规范。但是如果音视频的数据是以原始数据的形式存放的则这其占用的存储空间会很大，特别是对于视频数据来说。一段时长为5分钟24帧每秒的UHD 4K的SDR YUV420原始视频数据其将占用83GB的数据，如此巨大的体积显然让钱包无法接受。所以通常我们需要对原始的数据进行压缩编码以缩小数据大小节省存储空间。压缩编码还被细分为两种编码类型无损编码和有损编码，无损编码的多媒体数据可以被还原为压缩前的原始数据来回放，有损编码的多媒体数据则无法被还原为压缩前的原始数据，其压缩是不可逆的。音频数据有损压缩和无损压缩均较为常见，但是常见的视频压缩方法均为有损压缩。常见的一些音视频编码格式如下：  

![](images/ffmpeg/code.png)  

<!-- 以上编码格式的介绍 -->

<!-- 
### 图像
![](images/ffmpeg/CIE1931xy_blank.svg "Public domain")
![](images/ffmpeg/CIE1931xy_sRGB.svg "Public domain")
![](images/ffmpeg/CIExy1931_CIERGB.png)
![CIE1931xy_gamut_comparison.svg](images/ffmpeg/CIE1931xy_gamut_comparison.svg "BenRG and cmglee, CC BY-SA 3.0, via Wikimedia Commons")

### 视频
![IE1931xy_gamut_comparison_of_sRGB_P3_Rec2020.svg](images/ffmpeg/CIE1931xy_gamut_comparison_of_sRGB_P3_Rec2020.svg "CC BY-SA 4.0, via Myndex")
### 音频
 -->

## ffmpeg
### ffmpeg架构说明
<!-- opening? -->
Demuxer（解封装器）用于解开容器的封装获取其中的流。
![](images/ffmpeg/demuxer.png)  

Muxer（封装器）用于将流封装进容器中。
![](images/ffmpeg/muxer.png)  

Decoder（解码器）用于将流解码为原始数据。
![](images/ffmpeg/decoder.png)  

Encoder（编码器）用于将原始数据编码为流。
![](images/ffmpeg/encoder.png)  

注：本文有时会将原始数据称作原始数据流，甚至亦将其简称为流。故请读者根据语境判断“流”指的是编码数据流还是原始数据流。

ffmpeg提供了许多滤镜（Filter）用于对原始数据流进行处理。我们可以将多个滤镜组合为一个处理链路，我们将此链路称为滤镜图（filtergraph）。滤镜图按照其输入输出流的数量可以分为两类滤镜图。  

一进一出的滤镜图被称为简单滤镜图（simple filtergraph）。
![](images/ffmpeg/s-filter.png)  
一进多出、多进一出、多进多出的滤镜图则被称为复杂滤镜图（complex filtergraph）。  
![](images/ffmpeg/c-filter.png)  

将上文所述的组件组合在一起，即可对多媒体文件（流）进行各式各样的处理。

![](images/ffmpeg/process0.png)  
将INPUT.mkv的流1封装为OUTPUT.mp4。  
其效果为将INPUT.mkv的流1提取为OUTPUT.mp4。  

![](images/ffmpeg/process1.png)  
将INUPUT0.mkv的流0和INPUT1.aac的流0封装到OUTPUT.mp4中。  
其效果为提取INUPUT0.mkv和INPUT1.aac各自唯一的流，音视频合并到OUTPUT.mp4中。  

![](images/ffmpeg/process2.png)  
将INPUT.mkv的流0封装成OUTPUT0.mp4，流1封装成OUTPUT1.mp4。  
其效果为将INPUT.mkv的流0提取为OUTPUT0.mp4，流1提取为OUTPUT1.mp4。  

![](images/ffmpeg/process3.png)  
将INPUT.mkv的流0（音频流）封装到OUTPUT.mp4中，流1（视频流）则使用视频解码器将其解码，再使用libx264库编码为x264编码的视频流，再一并封装到OUTPUT.mp4中。  
其效果为将INPUT.mkv视频转码成H.264编码后装到OUTPUT.mp4中，音频则原样复制到OUTPUT.mp4中。（即，音频保持原样不变视频转码为H.264）

![](images/ffmpeg/process4.png)  
将INPUT.mkv的视频流解码再使用一个简单滤镜图对视频流进行处理，滤镜图使用yadif反交错滤镜处理视频，再使用scale滤镜缩放视频。之后将处理后的视频流编码再封装到OUTPUT.mp4中。  
其效果为对隔行扫描的视频（如1080i的视频）进行反交错处理并进行缩放，得到一个画面大小合适的逐行扫描视频（如2160p的视频）。  

![](images/ffmpeg/process5.png)  
<!-- 待添加描述 -->

### ffmpeg基础用法
ffmpeg命令的语法为
<!-- 调整了ffmpeg语法中表示输入输出文件名称，以方便后续的说明 -->
```
ffmpeg [global_options] {[input_file_options] -i inupt_file} ... {[output_file_options] [output_file]} ... 
```
输入文件和输出文件既可以是普通的文件路径也可以是URL还可以是捕获/回放设备的路径。

ffmpeg通过选项`-i`输入文件，例如要输入视频文件`input.mp4`则可以使用选项`-i input.mp4`。如要使用多个输入文件则可以使用多个`-i`选项来指定，如：`-i input1.mp4 -i input2.m4a`。在输入多个文件时要注意，通过`-i`选项输入文件的先后顺序将决定输入文件的索引，第一个输入文件的索引为0、第二个为1、第三个为2以此类推。  

下面是一条最简单的ffmpeg命令
```sh
ffmpeg -i file.mp4 item.flv
```
此命令可以把输入文件file.mp4转换为flv格式的输出文件item.flv，ffmpeg可以通过输出文件的后缀名自动判断目标格式。

但上面的命令其实潜藏着一个坑——如果不使用选项指定编码器，则ffmpeg会对输入文件的所有输入流<sup>注</sup>进行转码，流的编码将转换为目标文件格式对应默认流编码（使用相应的默认编码器）。即使输入文件中流的编码和目标文件的默认编码一致也会重编码。重编码为有损格式将会损失媒体的细节，且重编码的过程会占用大量的计算机资源并会耗费一定时间。对于mp4, mkv, flv这三个常见视频容器格式来说其默认的视频流编码格式为H.264，音频流编码格式为AAC。

注：在本文中，除非专门提及，否则附件（流）不在讨论范围内。

如果只是要转换文件（容器）格式而不需要对流进行转码则需要使用`-codec copy`选项来让ffmpeg直接复制流数据而不转码。
```sh
# copy only
ffmpeg -i file.mp4 -codec copy item.flv
```
命令中的`-codec`选项通常<!-- 此处选用“通常”一词 -->用于指定编码器，对所有流指定使用`copy`编码器（仅复制不转码）。

<!-- 音视频copy only合并 -->
有的视频的音视频分别存放在两个文件中，而我们可以通过ffmpeg将其音频和视频合并为一个有音频的视频。
```sh
ffmpeg -i video.mp4 -i audio.m4a -codec:v copy -codec:a copy outupt.mp4
```
在上面的命令中没有使用`-codec copy`，而是使用`-codec:v copy`将视频流的编码器指定为`copy`编码器，使用`-codec:a copy`将音频流的编码器指定为`copy`编码器。所以`-codec:v copy -codec:a copy`的效果和`-codec copy`的效果是类似的。在`-codec:v`和`-codec:a`中的`:v`和`:a`则分别说明了指定的是视频流和音频流，其中的`v`和`a`都是一个流指定器（stream specifier）。

流指定器可用于指定一个或多个流。流指定器最常见的形式是`stream_index`和`stream_type[:stream_index]`，其中stream_index是表示流索引<sup>注</sup>的数字，stream_type则是表示流类型的标识符v表示视频、`a`表示音频、`s`表示字幕、`t`表示附件、`d`表示数据。流指定器`stream_index`将指定对应索引的流，如`0`指定流索引为0的流。`stream_type`则会指定所有对应类型的流，如`v`将指定所有视频流。而形如`stream_type:stream_index`的流指定器将指定的流类型中指定索引的流，如`a:1`表示音频流中音频流索引为1的流（即第二个音频流）。

<!-- 注：关于流索引的说明 - 查看方法与编号规则-->
注：流索引表示的是一个文件内各个流的序号，流索引从0开始编号。通过命令`ffprobe media_file`可以查看各个流的信息及其索引。

<!-- 特定轨道分离提取 -->
假如要提取一个stream 0为视频流stream 1为flac格式的音频流的媒体文件的音频流可以使用命令
```sh
ffmpeg -i media.mkv -map 0:1 -codec copy audio.flac
```
其中`-map`选项用于将指定流映射到相应输出中，通俗来说就是将选中的流添加到相应的输出文件中。`-map`选项的简化语法为`-map [-]input_file_id[:stream_specifier]`<!-- 尾随的?因语法存疑暂时不讨论 -->，其选项参数`[-]input_file_id[:stream_specifier]`用于指定被映射的（一个或多个）输入流，其中的`input_file_id`即为前文提到的输入文件索引。<!-- 待调整此处说明 -->所以选项参数`input_file_id`将指定相应输入文件的所有流，而`input_file_id:stream_specifier`将指定相应输入文件中流指定器所指定的（一个或多个）流。假如ffmpeg命令中没有使用`-map`选项则将根据以下规则进行自动流选择  
1. 如果存在视频流，选择分辨率最高的视频流。
2. 如果存在音频流，选择声道数最多的音频流。
3. 如果存在字幕流，选择第一个字幕流。
4. 如果多个同类型的流级别一致，则选择相应类型中流索引最小的流。

注意到，在`-map`的选项参数中还存在一个可选的前缀`-`，`-`的作用是“负”映射，即排除/取消映射。负映射不能独立使用，必须要使用了常规的映射才能使用负映射（但没要求输入文件用了常规映射的才能用负映射）。负映射可用于排除已选择映射中的指定映射（一个或多个）。例如有一个视频文件其中stream 0是视频流、stream 1-3为音频流、stream 4-6为字幕流，使用命令
```sh
ffmpeg -i movie.mkv -map 0:a -map -0:1 -codec copy out.mka
```
将选中所有音频流，排除流stream 1，根据选择与排除最后被映射的流为0:2和0:3。要特别注意，`-map`的选项参数包含了输入文件索引，这意味着无论是常规映射还是负映射其选择或排除的流索引对应的都是相应输入文件的原始流索引，而不是前一个map选剩的残羹剩饭。除此之外还要再提一点，假如负映射排除的是常规映射没包含过的流，命令仍然可以正常执行。这个行为可能会让有的人感到莫名其妙。

如果要排除特定类型的流还可以使用选项`-vn`、`-an`、`-sn`、`-dn`用于排除所有视频流、音频流、字幕流、数据流。且这四个选项和负映射不同，其可以脱离常规映射单独使用。

通过`-map`选项我们可以分离一个视频的视频流和音频流。
```sh
ffmpeg -i media.mp4 -map 0:0 -codec copy videonly.mp4 -map 0:1 -codec:a copy audionly.aac
```
此命令使用了`-map`和`-codec`选项将音视频流直接复制到了合适的容器中。

<!-- 特定轨道转码及单轨文件转码 -->
有时，我们要对多媒体文件的特定轨道（流）进行转码则可以使用以下命令
```sh
ffmpeg -i video.AVC.mp4 -codec:v libsvtav1 -codec:a copy out.av1.mkv
```
在此命令中`-codec:v libsvtav1`选项将视频编码器指定为libsvtav1，视频流将被软件编码为AV1编码，音频流则原样复制。`-codec`指定的编码器将决定编码成的编码格式，ffmpeg为有的编码提供了多个编码器实现，如前文使用的libsvtav1就是ffmpeg提供的数个AV1编码器中的一个。我们可以根据需要选择合适的编码器来将流编码成目标格式。

对只有一条流的媒体文件进行转码则更加简单，下面的命令用于将wav音频转码为flac音频。
```sh
ffmpeg -i music.wav -codec:a flac output.flac
```
在该命令中音频编码器被指定为flac，正如其名此编码器用于将音频流编码为flac编码。

若要查看当前ffmpeg支持的编码器可以使用命令
```sh
ffmpeg -encoders
```

<!-- codec选项的澄清 -->
在前文中`-codec`选项均用作指定编码器，但正如该选项的字面意思所示(en)**co**der**dec**oder，其亦可用作input_file_options来指定解码器。例如命令
```sh
ffmpeg -codec:v vp9 -i file.vp9.mkv -codec:v libx264 -codec:a copy out.mp4
```
使用了输入选项`-codec:v vp9`将视频流将解码器指定为`vp9`软件解码器。然后使用输出选项`-codec:v libx264`将视频流的编码器指定为`libx264`软件编码器并使用输出选项`-codec:a copy`来直接复制音频流。

在ffmpeg的命令中如果没有在输入文件选项中指定解码器则通常会自动选择合适的原生软件解码器<sup>注1</sup>，但如果输出到output_file的流的编码器设置为`copy`则在这一条（或多条）从输入到输出的数据传输链路上将不会执行任何编解码操作。

如要查看当前ffmpeg支持的解码器可以使用命令
```sh
ffmpeg -decoders
```

<!-- 注1:什么是原生解码器 todo -->

<!-- 注：选项别名 -->
补充说明：ffmpeg为他的部分选项提供了多个作用相同的名字，在此称其为“选项别名”。例如`-codec`选项的另一个写法为`-c`，所以我们可以将前文介绍过的命令
```sh
ffmpeg -codec:v vp9 -i file.vp9.mkv -codec:v libx264 -codec:a copy out.mp4
```
改写为
```sh
ffmpeg -c:v vp9 -i file.vp9.mkv -c:v libx264 -c:a copy out.mp4
```
又比如选项`--help`他还可以写作`-h`, `-halp`, `-?`。本文不会过多的介绍选项别名，如要了解选项别名的相关信息请自行查阅ffmpeg的官方手册。

<!-- 视频硬件加速转码之硬件编解码器加速 -->
在前文的示例中，对视频流进行转码时使用的编码器均为软件编码器，软件编码器的计算均交由CPU执行，虽然软件编码的质量较高但是编码速度较慢。为了提升转码的编码速度我们可以使用GPU中的硬件编解码器来对视频进行硬件编码。

> 在笔者的Intel Core i5 1240P+NVIDIA RTX 3050(50W)笔记本中对1080P 10bit HEVC规格的动画进行重编码，libx265软件编码速度为2.1x。而使用hevc_nvenc编码器调用NVIDIA的编码单元进行编码的速度则高达15.5x。

若想使用GPU编码器加速硬件编码首先要安装好相应的GPU驱动和编解码依赖。

对于**NVIDIA**显卡只需[安装NVIDIA官方驱动](../improve/install-nv.md)即可（不建议安装Debian源中提供的驱动）。使用NVIDIA的编解码器加速编解码的体验是最好的。

对于**Intel**显卡配置过程相对繁琐。在Debian Trixie中，首先要安装Intel闭源显卡驱动`intel-media-va-driver-non-free`和`i965-va-driver-shaders`替换掉开源显卡驱动`intel-media-va-driver`和`i965-va-driver`，然后安装闭源固件`firmware-misc-nonfree`和`libmfx-gen1.2`Intel GPU编解码依赖库，最后重启计算机后才能使用Intel显卡的完整硬件编解码加速功能。但即便如此，在部分计算机上Intel显卡也无法解码部分其理应支持解码的视频流。在ffmpeg中使用Intel显卡加速编解码并不是一个好的选择。

对于**AMD**显卡根据资料所说，安装`mesa-va-drivers`后即可使用相应的硬件编解码功能。但笔者没有相应的测试平台无法确保此说法的有效性及AMD显卡硬件编解码的实际支持情况。

在配置好显卡驱动及编解码依赖后即可正常使用ffmpeg的硬件编码功能，各家显卡的各常用编码器在ffmpeg中的编码器名如下表所示：

|显卡\编码 | AV1       | H.265      | H.264      |
| -------- | :-------: | :--------: | :--------: |
| NVIDIA   | av1_nvenc | hevc_nvenc | h264_nvenc |
| Intel    | av1_qsv   | hevc_qsv   | h264_qsv   |
| AMD      | av1_amf   | hevc_amf   | h264_amf   |

注意，最终能使用哪些硬件编码器以显卡的技术规格为准。

在安装了NVIDIA RTX 5080的平台中使用NVIDIA编码器将H.264编码的蓝光原盘视频流转码为AV1的命令为
```sh
ffmpeg -i remux.mkv -c:v av1_nvenc -c:a copy -c:s copy compress.mkv
```
在此命令中音频流和字幕流原样复制，视频流编码器则指定为`av1_nvenc`，以使用NVIDIA显卡的编码器将视频流硬件编码为AV1。

GPU中除了提供硬件编码器外自然还提供了硬件解码器，在前文中笔者故意使用了“硬件编**解**码器”来提醒读者GPU不但提供了编码器还提供了解码器。实际上因为解码器相对于编码器更容易实现，所以当出现了一个新的视频编码格式，显卡厂商会先实现该编码硬件解码器，过一段时间后才能实现硬件编码器并将其添加在后续的产品中。因此对于某种视频编码显卡只提供了硬件解码器而没有提供编码器是很正常的。各家显卡的各常用解码器在ffmpge中的解码器名如下所示：

|显卡\编码 | AV1       | H.266   | H.265      | H.264      |
| -------- | :-------: | :-----: | :--------: | :--------: |
| NVIDIA   | av1_cuvid | -       | hevc_cuvid | h264_cuvid |
| Intel    | av1_qsv   | vvc_qsv | hevc_qsv   | h264_qsv   |
| AMD      | -         | -       | -          | -          |

注：`-`表示未提供或未知。和硬件编码器一样，最终能使用哪些硬件解码器以显卡的技术规格为准。

正常情况下一个转码过程（即编解码过程）中的瓶颈是显卡的硬件编码器而非软件解码器。但在一些特殊情形中解码器也有可能成为瓶颈，为了避免这种情况我们可以将解码器指定为硬件解码器以提升解码速度。命令
```sh
ffmpepg -c:v av1_cuvid -i media.av1.mkv -c:v h264_nvenc out.ave.mp4
```
将解码器指定为`av1_cuvid`以使用NVIDIA的硬件解码器解码AV1视频流以提升解码速度，进而提升整体的转码速度。

<!-- 视频硬件加速转码之硬件并行加速hwaccel -->
除了通过指定解码器加速硬件解码外还可以通过指定硬件加速API来加速解码。执行`ffmpeg -hwaccels`命令即可查看当前ffmpeg支持的加速接口，常见的接口有`cuda`、`vulkan`、`vaapi`等。而最终ffmpeg可正常使用的接口则取决于运行平台。ffmpeg使用`-hwaccel`选项来指定硬件加速接口。例如命令
```sh
ffmpeg -hwaccel cuda -i input.mp4 -c:v hevc_qsv output.mp4
```
使用了NVIDIA的`cuda`硬件加速接口进行解码，并使用`hevc_qsv`编码器进行编码。<!-- 这部分和“音视频码率控制与质量控制”部分及“像素格式转换”部分的内容实际上相当复杂，在简化版中就暂时介绍到这里。注：hwaccel_output_format选项的作用通过`ffmpeg --help full`查看 -->
<!-- todo: vulkan接口编码加速 -->

<!-- 音视频码率控制与大小控制 -->
有的媒体源的媒体流规格较高我们可能用不着（或不需要）这么高的规格，此时我们可能希望对源媒体进行码率<sup>注</sup>控制，以节省存储空间或减小带宽要求。在此前提下我们也自然希望画面保持相对较好的质量。

注：“码率”即比特率(bitrete)，意为每秒的比特数(bit pre second)。对于有损多媒体流，码率定义了媒体流每秒的预期大小。对于有损视频而言，码率会影响画面的精细度（在同一分辨率下，码率越高画面的精细度越高），对于有损音频来说码率决定了（心理声学意义上的）音质高低。但要注意，对于常见的有损音频格式来说，码率为320kbps的有损音频其音质已相当接近人耳的听觉极限。

对于视频的码率控制方式可以不严谨地分为CBR、ABR、VBR（Variable Bitrate，可变码率）、CQ。

CBR（Constent Bitrate，恒定码率）的码率为固定值，此码率控制方式容易造成欠码或码率浪费。如今此码率控制方式已较少使用。本文不再对CBR进行进一步的说明。

ABR（Average Bitrate，平均码率）亦称作目标码率（Target Bitrate）。ABR码率控制方式将尝试根据各个时刻画面的表现需要分配合适的码率并让最终流的码率为我们的目标平均码率。换言之，ABR的视频的码率是根据画面动态变化（分配）的，而不是和CBR那样一直为一固定值。

在ffmpeg中可以使用选项`-b:v <birtate>`将视频的码率控制方式设置为ABR。其选项参数则指定视频的码率，该选项参数的写法为`<uint>k`或`<uint>M`<!-- 改进此处的语法 -->。如命令
```sh
ffmpeg -i video.mp4 -c:a copy -c:v libx264 -b:v 5000k out.mp4
# equal to
ffmpeg -i video.mp4 -c:a copy -c:v libx264 -b:v 5M out.mp4
```
将视频流编码为H.264并尝试将视频的平均码率控制在5000kbps（即5Mbps）左右。

在此说明一下，选项`-b`的完整语法是`-b[:stream_specifier] <bitrate>`。但要注意`-b`选项流指定器中`stream_type`的值只能为`v`或`a`。如果流指定器指定的是一条特定的流，则设置的`bitrate`只会作用于该流。如果流指定器指定了多个流，`bitrate`设置的是每一条流的码率，而非多个流总和的码率。

<!-- CQ是h264、h265、av1的默认码率控制方式 -->
CQ（Constant Quality，恒定质量）常被称作CRF（Constant Rate Factor，恒定数率因子）。CRF码率控制方式将根据画面的需要动态分配码率使得视觉观感（画质）一致。如在一个影片中画面简单的片段分配合适的码率让其在视觉上**画质较好**，而在复杂的场景中为其分配足够的码率令该片段在视觉上亦**画质较好**。

在FFmpeg中目标编码格式为AV1、H.264、H.265时如果不指定码率控制方式则默认使用CRF。CRF使用一个在有效范围内的系数来控制视频在视觉上的观感。不同的编码甚至不同的编码器都具有不同的CRF取值范围、默认值及视觉上画质无损值，常见编码或编码器的CRF取值可参见下表：

| 编码或编码器 | libsvtav1(AV1) | libaom-av1(AV1) | H.264 | H.265 |
| - | - | - | - | - |
| CRF有效范围 | 0-63 | 0-63 | 0-51 | 0-51 |
| CRF默认值 | 35 | / | 23 | 28 |
| CRF视觉无损值 | / | 23 | 17 | 24 |

注：`/`表示未知。上表没有考虑AVC 10bit的情况。

在FFmpeg中使用`-crf <int>`选项来指定CRF系数，示例如下
```sh
ffmpeg -i src.mp4 -c:a copy -c:v libsvtav1 -crf 26 dest.mp4 
```
输入文件的视频流使用`libavtav1`编码器编码为AV1，CRF系数被设为26。

<!-- 2pass（应该放哪里？）
多遍编码 N pass。h264、av1和h265的参数/选项有所不同
```sh
# h264 & av1
ffmpeg -i video.mp4 -c:v <264_or_av1_enc> -b:v 10M -pass 1 -an -f null /dev/null
ffmpeg -i video.mp4 -c:v <264_or_av1_enc> -b:v 10M -pass 2 -c:a copy out.mp4

#h265
ffmpeg -i video.mp4 -c:v <265_enc> -b:v 10M -x265-params pass=1 -an -f null /dev/null
ffmpeg -i video.mp4 -c:v <265_enc> -b:v 10M -x265-params pass=2 -c:a copy out.mp4
```
 -->

<!-- 
```
-maxrate and vbv-maxrate
```
-->

<!-- 除了CBR其他码率控制方式实际上都属于VBR -->
澄清：上面介绍了那么多编码控制方式而实际上编码控制方式只分为两类：码率固定不变的CBR和码率根据画面动态分配的VBR。实际上除了CBR其他的码率控制方式全是VBR，但在不少技术资料中却将VBR及其细分的编码控制方式的关系整地相当混乱。例如有的资料淡化了VBR类的概念然后把他的细分编码控制方式ABR、CQ、VBR单独列出于CBR并列。你注意到了吧，后面怎么又跑了一个VBR出来，此VBR非彼VBR，一个指的是VBR类另一个指的是N pass，但N pass也可以叫作VBR。这都是啥呀这是！反正你只要知道在离开了标准文件的领域，在其他地方（包括较为权威的技术文档）这些编码控制方式的关系说明乱成一锅粥就是了。

<!-- 音频码率控制 -->
音频码率和质量的控制方式分为两种：无损音频的质量控制和有损音频的码率控制。

无损音频的数据量远远小于无损视频的数据量，所以使用无损音频是一件很常见的事。若想减少一个无损音频流的占用的存储空间可以将无压缩的音频编码为无损压缩音频，如将无压缩的WAV PCM音频编码为无损压缩的FLAC音频。
```sh
ffmpeg -i in.wav -c:a flac out.flac
```
也可以将一个高规格的无损音频转换为相对低规格的无损音频，可使用的方法为降低采样率或降低位深。如将位深<sup>注1</sup>为24位整型的音频转换为位深为16位整型的音频：
```sh
ffmpeg -i in.flac -sample_fmt s16 out.flac
```
其中`-sample_fmt`选项用于设置音频采样点的格式（采样点格式会决定位深），其语法为`-sample_fmt[:stream_specifier] sample_format`。通过`ffmpeg -sample_fmts`命令即可查看ffmpeg支持的音频采样点格式；或将采样率为48kHz的音频重采样为44.1kHz的音频：
```sh
ffmpeg -i in.flac -ar 44100 out.flac
```
其中选项`-ar[:stream_specifier] freq`用于设置音频采样率（即频率），但要注意无损音频的采样率不应低于44.1kHz<sup>注2</sup>。

注1：音频的位深将决定音频的动态范围（单位为dB），其计算公式为Dynamic Range = 20log<sub>10</sub>2<sup>depth</sup>，由此易得24Bit位深能提供高达144dB的动态范围，16Bit的位深也能提供96dB的动态范围，96dB足以胜任绝大多数音乐的回放了。8Bit位深只提供了少得可怜的48dB动态范围（注意，不要混淆！8Bit位深音频和8Bit音乐风格不是同一个东西）。

注2：根据奈奎斯特采样定律若要完整采样带限信号采样率应大于信号最高频率的2倍。因此对于音频信号来说，因为人耳的听觉频率范围20-20000Hz，所以无损音频的最高频率要至少覆盖到20000Hz，为了完整采样20000Hz的音频信号，采样率必须要>40000Hz。之后又因为用于防止信号混叠的低通滤波器无法达到理想情况中的完美截断，所以还需要保留一段过渡频段用于滤波器的高频滚降。在现实中无损音频的代表性标准CD音频标准中采样率最终设定为44.1kHz。故无损音频的采样率不应低于44.1kHz，低于44.1kHz就算不上无损了，但在音频回放中也没有必要追求过高的采样率，DVD Audio标准的48kHz采样率已经完全足够了。

注：CD Audio和DVD Audio规格如下表所示
| 媒介   | CD      | DVD    |
| ------ | ------- | ------ |
| 位深   | 16 bit  | 24 bit |
| 采样率 | 44.1kHz | 48kHz  |

有损音频通常通过码率来控制其音频质量。`-b:a bitrate`选项用于设置所有有损音频流<sup>注1</sup>的码率，如需让码率值作用于特定的音频流则需使用正确的流指定器形式指定特定流。下面的命令用于将一个flac无损音频文件转换为480kbps的ogg音频格式：
```sh
ffmpeg -i gris.flac -b:a 480k gris.ogg
```

注1：对于wave和flac无损音频应用-b:a选项在不同情况下效果各不相同，你只要知道不管码率是什么其输出音质不会比CD无损音质差。

注：各音质级别与常见有损音频编码格式码率的参照表可见文末附加信息段落。

<!-- 像素格式转换（含像素格式简介） -->
视频由多个帧组成，而帧由多个像素组成。但在视频中像素不是和彩图一样由RGB组成而是由YUV组成。严谨地来说YUV和RGB都是色彩空间，而形如YUV420p或RGB888才指的是特定的像素格式。YUV色彩空间中的Y分量是明度分量，U是蓝色色度分量，V是红色色度分量。而在计算机中常说的YUV实际上指的是YCbCr（本文也将遵循这一惯例）。在视频中最常用的YUV像素格式为yuv420p和yuv420p10le，yuv420p是最常见的8bit视频像素格式，yuv420p10le是最常见的10bit像素格式。

`ffmpeg -pix_fmts`命令可用于查看FFmpeg支持的像素格式。`-pix_fmt[:stream_specifier] format`选项则用于指定输入文件/输出文件的像素格式，但在实际上可用的像素格式则根据编解码器和输入输出格式的不同而不同。对于libx265编码器而言其可以使用`yuv420p`（8bit YUV）、`yuv420p10le`（10bit YUV420）等像素格式。对于NIA三家的H.265编码器而言其可以使用`yuv420p`（8bit YUV）、`p010le`<sup>注1</sup>（10bit YUV420）等像素格式。通过`ffmpeg -h encoder=encoder_name`命令可以查看编码器信息，其中会说明此编码器支持的像素格式。

下面的命令将一个AV1编码像素格式为10Bit YUV420的视频通过`hevc_nvenc`编码器编码为H.265编码像素格式为8Bit YUV420的视频：
```sh
ffmpeg -i input.av1.mp4 -c:v hevc_nvenc -pix_fmt yuv420p output.mp4
```
此命令通过使用`-pix_fmt`选项将输出文件的像素格式指定为`yuv420p`（8Bit YUV420格式）

注1：不严谨地说，`yuv420p10le`和`p010le`都是同一种10Bit YUV420像素格式。在输出10Bit YUV420像素时如果使用的是软件编码器要使用名字`yuv420p10le`，如果使用的是硬件编码器则要使用名字`p010le`。但严谨地来说......好吧笔者也不清楚`yuv420p10le`和`p010le`的细节。这两货的关系有点混乱资料也不好查，他们之间也许存在一点细微<!-- 内存布局上的 -->差别。<!-- p010le究竟是个什么鬼 -->

<!-- 特定轨道提取转码大杂烩（综合运用） codetv.sh-like -->
以下示例命令用于将各种格式的视频转换为兼容绝大多数设备的视频：
```sh
ffmpeg -i input.vdo -map 0:0 -map 0:1 -c:a aac -b:a 480k -c:v nvenc_h264 -pix_fmt yuv420p -b:v 6M output.mp4
```
其中两个`-map`选项分别选定了需要的视频流和音频流，音频流被编码为480kbps的AAC音频，视频流通过`nvenc_h264`编码为H.264格式的视频，码率使用ABR控制方式限制为6Mbps，且视频的像素格式被转换为`yuv420p`格式。

注：在大多数视频文件中各类流的排序顺序为视频、音频、字幕、附件及数据。如在一个有一个视频流、两个音频流、两个字幕流的视频文件中，其流顺序通常为视频流0、音频流0、音频流1、字幕流0、字幕流1。
<!-- 一些可能会用到的参数介绍：-r帧率、-f选项 - 本地文件推流&怪文件读取 -->

<!-- 简单滤镜图之反交错+内嵌字幕 （示例：fx先行版？） -->

<!-- 复杂滤镜图？ -->

### ffmpeg实用示例说明

m3u8下载及合并
```sh
ffmpeg -i <link2m3u8> -c copy out.mp4
```
<!-- 内封字幕移植 -->
<!-- 命令行推流直播 -->

## ffplay
ffplay是ffmpeg中包含的一个基于ffmpeg的简易播放器实现。但是要注意ffplay渲染的视频画质存在些许问题，不建议将其用作日常使用的播放器。
### ffplay的基本使用说明
使用ffplay播放多媒体时使用`←`可向前跳转10秒，使用`→`可向后跳10秒，使用`↓`可向前跳转一分钟，使用`↑`可向后跳转一分钟  
播放多媒体文件
```sh
ffplay <media_file>
```
<!-- 摄像设备播放 -->
### 稍复杂的ffplay示例说明

## ffprobe
ffprobe用于获取并显示多媒体的信息。通过如下命令即可查看多媒体信息。  
```sh
ffprobe <media_file>
```
<!-- 获取特定信息 -->

## 附加信息
<!-- 高质量视频推荐码率参考表 -->
<!-- 立体声有损音频各编码码率音质参照表 -->
### 立体声有损音频码率音质参照表 
| 音质\音频编码格式 | AAC | Opus | Ogg Vorbis | MP3 |
| ----------------- | - | - | - | :-: |
| 能听              |  |  |  | 128k |
| 中音质            | 128k |  | 128k | 192k |
| 高音质            |  |  | 256k | 320k |
| 近乎无损          | 320k | <!--略小于-->320k |  | X |
<!-- todo -->
<!-- 常见软件编码器 -->
<!-- 常见软件解码器？ -->

<!-- 完整版待讨论项 -->
<!-- 编码硬件和编码软件与画质的关系 -->
<!-- 从RGB色彩模型到YUV色彩模型 -->

## ref
https://ffmpeg.org/about.html  
https://ffmpeg.org/pipermail/ffmpeg-devel/2006-February/010315.html
<!-- https://en.wikipedia.org/wiki/CIE_1931_color_space
https://commons.wikimedia.org/wiki/File:CIE1931xy_blank.svg
https://commons.wikimedia.org/wiki/File:CIE1931xy_sRGB.svg
https://commons.wikimedia.org/wiki/File:CIExy1931_CIERGB.png
https://commons.wikimedia.org/wiki/File:CIE1931xy_gamut_comparison.svg -->
http://docs-hoffmann.de/ciexyz29082000.pdf  
https://ffmpeg.org/ffmpeg.html
https://ffmpeg.org/ffmpeg-all.html  
特殊资料代号pfv   
深入理解FFmpeg - 刘歧；赵军；杜金房；赵文杰；宋韶颍 ISBN: 9787115621368  
https://github.com/Diercohen/m3u8-downloader/blob/main/install.sh
<!-- https://en.wikipedia.org/wiki/Rec._709
https://en.wikipedia.org/wiki/Rec._2020 -->
https://en.wikipedia.org/wiki/Matroska
https://www.cnblogs.com/guanghe/p/6277118.html  
FFMPEG(1)  
POSIX 2024 Volume 1 Chapter 12 Utility Conventions  
https://wiki.debian.org/HardwareVideoAcceleration
https://bugxia.com/3817.html
https://en.wikipedia.org/wiki/NVENC
https://en.wikipedia.org/wiki/NVDEC
https://en.wikipedia.org/wiki/Intel_Quick_Sync_Video
https://en.wikipedia.org/wiki/Video_Core_Next
https://en.wikipedia.org/wiki/Video_Coding_Engine
https://docs.nvidia.com/video-technologies/video-codec-sdk/13.1/ffmpeg-with-nvidia-gpu/index.html  
https://developer.nvidia.com/video-encode-decode-support-matrix  
https://trac.ffmpeg.org/wiki/HWAccelIntro  
`ffmpeg --help full`  
https://trac.ffmpeg.org/wiki/FFprobeTips
https://trac.ffmpeg.org/wiki/Encode/H.264
https://trac.ffmpeg.org/wiki/Encode/H.265
https://trac.ffmpeg.org/wiki/Encode/AV1  
https://slhck.info/video/2017/03/01/rate-control.html
https://www.cnblogs.com/mfrbuaa/p/3856187.html  
Toole, F.E. (2017). Sound Reproduction: The Acoustics and Psychoacoustics of Loudspeakers and Rooms (3rd ed.). Routledge. ISBN 9781315686424  
https://slhck.info/video/2017/02/24/vbr-settings.html  
https://ffmpeg.org/ffmpeg-codecs.html   
https://ffmpeg.org/index.html#news  
https://wiki.videolan.org/YUV/  
https://en.wikipedia.org/wiki/Y%E2%80%B2UV
https://en.wikipedia.org/wiki/YCbCr
https://support.google.com/youtube/answer/1722171  
https://www.cnblogs.com/zuojie/p/16756046.html  
https://handwiki.org/wiki/Nyquist%E2%80%93Shannon_sampling_theorem
https://handwiki.org/wiki/44,100_Hz  
[为何最优秀的OGG却变成最小众的，OGG，MP3,AAC音频比较？ - 知乎用户yINU3y的回答 - 知乎](https://www.zhihu.com/question/296018289/answer/497917102)
  
https://en.wikipedia.org/wiki/Comparison_of_audio_coding_formats  
https://en.wikipedia.org/wiki/Advanced_Audio_Coding  
https://en.wikipedia.org/wiki/Dolby_Digital_Plus  
https://en.wikipedia.org/wiki/Comparison_of_audio_coding_formats  
https://en.wikipedia.org/wiki/Opus_(audio_format)  
https://en.wikipedia.org/wiki/Vorbis