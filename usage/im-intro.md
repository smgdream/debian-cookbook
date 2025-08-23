# 输入法介绍

## 输入法框架
输入法框架（Input Method framework, IMF）用于为应用程序提供一个统一的输入法调用接口并且让用户可以方便地切换输入法。  

当前Linux世界中存在着两大流行的输入法框架分别是ibus和fcitx5（还有一个稍老的fcitx）。IBus对各种软件和输入法引擎的支持教好，下文仅对基于IBus框架的输入法编辑器进行讨论。Fcitx5支持的输入法编辑器稍少，如果需要使用[搜狗输入法Linux版](https://shurufa.sogou.com/linux)则需要使用Fcitx，而各应用对Fcitx系列的输入法框架支持稍差，部分应用使用Fcitx系列IMF会出现bug或无法使用。  

Debian Trixie IMF预装规则：如果安装Debian时选择非中文语言则仅安装Ibus（但不会安装`ibus-libpinyin`），如果选择中文则安装IBus和Fcitx5。  

注意：请确保系统中仅存在一个输入法框架，否则可能产生冲突导致不可预知的问题。  

## 输入法编辑器
上文讨论的输入法框架并非我们所熟知的输入法，我们熟知的输入法的实际名称为“输入法编辑器”(Input Method Editor, IME)（如所谓的搜狗输入法是一个输入法编辑器而非输入法框架）。  

**常用IBus输入法编辑器**  
| 名称 | 包名 | 描述 |
| --- | --- | --- |
| RIME | `ibus-rime` | RIME（中州韵）输入法引擎，支持五笔拼音粤拼双拼等多种输入方案（详见：[中州韻輸入法 RIME](../todo.md)） |
| 智能拼音 | `ibus-libpinyin` | IBus智能拼音（中文语言环境下gnome默认安装） |
| SunPinyin | `ibus-sunpinyin` | 由SUN公司开发的基于统计语言模型(SLM)的输入法引擎，理论上会有较好的智能输入体验（除非你的内容主打一个活字乱刷） |
| IBus table | `ibus-table` | 形码输入法引擎框架，支持五笔仓颉郑码等多种形码输入方案，但需要安装相应子包（如五笔安装`ibus-table-wubi`） |
| IBus拼音 | `ibus-pinyin` | 早期IBus拼音输入法（已停更多年） |

## 使用输入法
对于GNOME用户，打开gnome设置点击“键盘”->“输入源”->“添加输入源”之后后选择对应输入法语言（如：汉语中国），然后即可选择要启用的输入法。  

如果当前存在多输入法顶栏中将显示输入法指示器图标，点击图标将出现输入法指示器菜单，通过该菜单即可切换输入法。也可以通过`Super + Space`快捷键切换输入法（`Super`键即Win键、徽标键）。  


## 参考资料

\[1\] [Input method - ArchWiki](https://wiki.archlinux.org/title/Input_method)  
\[2\] [Linux的输入法框架](https://zhuanlan.zhihu.com/p/384171267)  
\[3\] [搜狗输入法linux-安装指导](https://shurufa.sogou.com/linux/guide)  
\[4\] [Sun Pinyin developers](https://www.chinadaily.com.cn/video/2010-09/16/content_11310174.htm)  
\[5\] [Red Hat Enterprise Linux 9 Getting started with the GNOME desktop environment Chapter 7. Enabling Chinese, Japanese, or Korean text input 7.4.5](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/getting_started_with_the_gnome_desktop_environment/assembly_enabling-chinese-japanese-or-korean-text-input_getting-started-with-the-gnome-desktop-environment#proc_switching-the-input-method-in-gnome_assembly_enabling-chinese-japanese-or-korean-text-input)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.2 | Date: 2025-08-20



