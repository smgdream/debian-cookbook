# Debian Trixie Bug报告

## 1. 部分环境中GTK4应用汉字渲染异常
问题描述：在100%显示缩放1.00字体缩放的前提下，gtk4应用中部分控件中部分汉字存在渲染异常的情况（如“色”字底下被截去，“最”字顶上被截去）。  
![gtk4 text render error image](images/bug-gtk4-text-render-err.png)  
问题定位：经过本人、朋友及网友的努力，现已基本确定该bug来源于上游gtk4。  
修复状态：未修复  

## 2. 较差的NTFS文件系统支持
问题描述：Debian trixie的NTFS文件驱动由ntfs-3g更换为了ntfs3，导致的NTFS文件系统较易出现因superblock存在问题而无法挂载。  
问题定位：问题应该来自于ntfs3驱动。  
缓兵之计：所有Windows或Windows PE的磁盘检查工具扫描并修复磁盘。
修复状态：未修复  

## 3. 应用文件夹偶尔无法正常关闭
问题描述：偶尔会遇到gnome应用文件夹无法通过点击应用文件夹外的空白区域关闭（除了dock栏和顶栏）。  
问题定位：在manjaro中也存在相似情况，故推测该bug源自于上游。  
修复状态：似乎已于rc2中修复（不确定是上游提供的修复还是debian提供的修复）  

---
License: CC0 | Version: 0.6.5 | Date: 2025-07-19
