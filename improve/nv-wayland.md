# NVIDIA驱动使用wayland显示服务

当你安装完nvidia驱动后你可能会发现显示服务变成了x11，而显示管理器中也没有了wayland桌面的选项。如果还需要使用wayland则需要对系统进行配置。配置方法如下：  

通过以下命令启用内核nvidia驱动的模式设置：  
```sh
echo 'GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX nvidia-drm.modeset=1"' > /etc/default/grub.d/nvidia-modeset.cfg
update-grub
```
说明：要在内核被调用时传递参数`nvidia-drm.modeset=1`，故可以通过修改/etc/default/grub文件或在/etc/default/grub.d目录中添加子grub配置文件，以在通过`update-grub`命令重新生成grub启动配置时将内核参数写入到启动配置中。  

然后通过以下命令禁用原本的gdm规则以启用gdm的wayland桌面选项：  
```sh
mv /usr/lib/udev/rules.d/61-gdm.rules /usr/lib/udev/rules.d/61-gdm.rules.bak
```
最后，重启计算机并在gdm中选择wayland桌面环境即可使用wayland桌面环境。  

## 参考资料

 \[1\] [Wayland - NvidiaGraphicsDrivers - Debian Wiki](https://wiki.debian.org/NvidiaGraphicsDrivers#Wayland)  
 \[2\] [Gnome使用nvidia闭源驱动启用wayland](https://worisur.github.io/2024/08/18/2024-08-18.Gnome%E4%BD%BF%E7%94%A8nvidia%E9%97%AD%E6%BA%90%E9%A9%B1%E5%8A%A8%E5%90%AF%E7%94%A8wayland/)  
 \[3\] [NVIDIA 驱动和 GNOME 和 Wayland](https://sh.alynx.one/posts/NVIDIA-GNOME-Wayland/)  
\[4\] [Understanding nvidia-drm.modeset=1 (NVIDIA Linux driver modesetting)](https://forums.developer.nvidia.com/t/understanding-nvidia-drm-modeset-1-nvidia-linux-driver-modesetting/204068/2)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.6.9 | Date: 2025-07-27
