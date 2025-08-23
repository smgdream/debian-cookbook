# 雾凇拼音
雾凇拼音(rime-ice)是一个较常用的RIME拼音输入方案，其亦提供了一个较为完善的词库（包含：中文、英文，emoji）。  

## plum安装雾凇拼音
在plum目录中执行以下命令即可安装rime。  
```
bash rime-install iDvel/rime-ice:others/recipes/full
```
注：关于plum的下载与使用见[RIME](rime.md)“东风破 plum”段落。  
然后在输入法指示器菜单处*重新部署*即可使用雾凇拼音。  

## 手动安装雾凇拼音
从[雾凇拼音github发布页](https://github.com/iDvel/rime-ice)下载最新的full.zip，然后将压缩包解压到RIME配置目录（注意：部分解压软件解压后将产生以压缩包名字命名的包含压缩包内容的文件夹full，将该文件夹复制到RIME配置目录是无法使用雾凇拼音的，要复制的是full文件夹内的文件和文件夹），之后在RIME配置目录中新建一个名为`default.custom.yaml`的配置文件并在其中写入一下内容：  
```
patch:
  __include: rime_ice_suggestion:/
```
然后注销并重新登录桌面环境或重启计算机即可使用雾凇拼音。  

## 配置雾凇拼音
与配置rime类似，当聚焦于文本框时通过`F4`快捷键打开设置候选框后选择雾凇拼音再选择对应项即可改变相应配置。  

### 删除不需要的词库
直接删除对应RIME配置文件夹内的对应词库文件夹即可，中文：`cn_dicts`、英文：`en_dicts`、emoji：`opencc`。  
注：如果需要删除英文词库还需要额外删除RIME配置目录中的`melt_eng.dict.yaml`配置文件，否则会出现ERROR。  

### 更新词库
在[雾凇拼音github发布页](https://github.com/iDvel/rime-ice)下载相应的最新词库压缩包并解压替换相应的旧词库文件夹即可。  

## 参考资料
\[1\] [Rime 配置：雾凇拼音](https://dvel.me/posts/rime-ice/)  
\[2\] [雾凇拼音 README](https://github.com/iDvel/rime-ice/blob/main/README.md)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5 | Date: 2025-08-20
