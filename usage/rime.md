# RIME

中州韵输入法引擎(RIME)是一个跨平台的输入法算法框架，需搭配输入法方案才能使用输入法，RIME既支持音码输入方案也支持形码输入方案。

## 安装RIME

注：后文仅讨论基于IBus的RIME的安装与使用

通过以下命令安装RIME
```sh
apt install ibus-rime
```
然后可在gnome设置中的相应位置启用输入法（见：[输入法介绍](im-intro.md)使用输入法段落），之后即可通过输入法指示器或`Super+Space`快捷键切换输入法。

## RIME的使用
rime默认自带了几种输入方案，当当前聚焦于文本框时可通过`F4`即可呼出输入方案选择菜单。

## RIME的配置
RIME的~配置目录如下：
IBus: `~/.config/ibus/rime`
Fcitx5: `~/.local/share/fcitx5/rime`

RIME输入法默认的候选词排布为纵向排布，如需横向排布则需将`~/.config/ibus/rime/build/ibus_rime.yaml`配置文件中的`horizontal`对应的值更改为`true`。注意：该方法仅适用于ibus-rime。

### 东风破 plum
plum是rime的输入法配置管理器。源仓库中没有提供该软件，需要从官方github仓库中下载使用。下载命令如下：  
```sh
git clone https://github.com/rime/plum.git plum
```
使用方法如下：  
```sh
cd ~/plum
bash rime-install <recipe_name>
```
关于plum的具体使用说明见：https://github.com/rime/plum   

## 参考资料

\[1\] [RIME | 中州韻輸入法引擎](https://rime.im/)  
\[2\] [下載及安裝 | RIME | 中州韻輸入法引擎](https://rime.im/download/)  
\[3\] [请问ibus-rime如何设置输入框横排显示](https://github.com/rime/ibus-rime/issues/52)  
\[4\] [雾凇拼音 README](https://github.com/iDvel/rime-ice/blob/main/README.md)  
\[5\] [東風破 /plum/](https://github.com/rime/plum/blob/master/README.md)  


---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.1 | Date: 2025-08-20
