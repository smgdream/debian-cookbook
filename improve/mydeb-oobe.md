# 本人的Debian初始化配置

首先按照[Debian初步配置](../start/init-conf.md)配置好后执行以下改进了4年的oobe脚本（至于为什么叫oobe，因为我以前是捣鼓Windows的）。  

```sh
#!/bin/bash
# readly for debian 13
# Version: 2025-08-10

echo user:
read USER_NAME
echo "Your user name: ${USER_NAME}"

dpkg --configure -a
apt update --fix-missing

apt remove baobab \
	   evince \
	   evolution \
	   fcitx5 \
	   gnome-calculator \
	   gnome-calendar \
	   gnome-characters \
	   gnome-clocks \
	   gnome-contacts \
	   gnome-keyring \
	   gnome-maps \
	   gnome-music \
	   gnome-snapshot \
	   gnome-software \
	   gnome-sound-recorder \
	   gnome-tour \
	   gnome-weather \
	   goldendict \
	   goldendict-ng \
	   libreoffice-* \
	   malcontent \
	   shotwell \
	   simple-scan \
	   totem \
	   yelp
apt remove mawk

apt autoremove
apt autoclean
apt clean

apt update
apt upgrade
	

# install apt tools
apt install aptitude apt-file apt-rdepends

# install useful software
# utilities
apt install vim ranger ncdu gawk
# disk
apt install gdisk gparted kpartx
# others
apt install inotify-tools qoi   

# install framebuffer utilities
apt install fbi fbterm fbset fbcat

# install network software
apt install wget wget2 aria2

# install information viewing tools
apt install iotop htop btop fastfetch

# install system backup tool
apt install timeshift

# install graphy software
apt install intel-gpu-tools vainfo nvtop
	
# install other software
apt install grub-efi-amd64
	
# install systme tools
apt install dconf-editor

# install gnome tools
apt install sassc

# add extern multimedia repository
wget https://mirrors.ustc.edu.cn/deb-multimedia/pool/main/d/deb-multimedia-keyring/deb-multimedia-keyring_2024.9.1_all.deb
apt install ./deb-multimedia-keyring_2024.9.1_all.deb
rm ./deb-multimedia-keyring_2024.9.1_all.deb
cat > /etc/apt/sources.list.d/dmo.sources << EOF
Types: deb
URIs: https://mirrors.ustc.edu.cn/debian-multimedia/
Suites: trixie
Components: main non-free
Signed-By: /usr/share/keyrings/deb-multimedia-keyring.pgp
EOF
# apt modernize-sources
apt update
apt dist-upgrade

# install multimedia software
apt install ffmpeg vlc nomacs
	
# install useful multimedia tools
apt install ffmpegthumbnailer

# install chinese pingying input method
apt install ibus-rime

# install developement enviroment
apt install gcc-14 gcc make cmake meson build-essential perl git python3
apt install linux-headers-$(uname -r | sed 's/.*-//g')

# install java developement enviroment
apt install openjdk-21-jdk openjfx #openjdk-25-jdk 

# install nvidia driver pre-install enviroment
apt install dkms pkg-config libglvnd-dev

# enable wayland for nvidia driver
#echo 'GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX nvidia-drm.modeset=1"' > /etc/default/grub.d/nvidia-modeset.cfg
#update-grub
#mv /usr/lib/udev/rules.d/61-gdm.rules /usr/lib/udev/rules.d/61-gdm.rules.bak
# Reference:
# https://wiki.debian.org/NvidiaGraphicsDrivers#Wayland
# https://worisur.github.io/2024/08/18/2024-08-18.Gnome%E4%BD%BF%E7%94%A8nvidia%E9%97%AD%E6%BA%90%E9%A9%B1%E5%8A%A8%E5%90%AF%E7%94%A8wayland/
# https://sh.alynx.one/posts/NVIDIA-GNOME-Wayland/

# install appimage dep library
apt install libfuse2t64

# install useful dev tools
apt install flex bison cppcheck gettext cloc

# install useful dev library
# mathematical
apt install libgmp-dev libmpfr-dev libmpc-dev libisl-dev
# other
apt install libncurses-dev libelf-dev libssl-dev

# install graphic dev library
apt install libgl-dev libx11-dev libwayland-dev

# install androd debug tool
apt install adb
	
#install audio tool
apt install helvum

#install gnome shell extentsions
apt install gnome-shell-extensions \
	gnome-shell-extensions-extra \
	gnome-shell-extension-user-theme \
	gnome-shell-extension-dashtodock \
	gnome-shell-extension-blur-my-shell \
	gnome-shell-extension-status-icons

#install a gtk4 terminal
apt install ptyxis
apt remove  gnome-terminal
	
apt remove yelp -y

apt autoremove
apt autoclean
apt clean
aptitude purge "~c"

# remove useless .desktop
rm /usr/share/applications/ranger.desktop
rm /usr/share/applications/vim.desktop
rm /usr/share/applications/htop.desktop
rm /usr/share/applications/btop.desktop
rm /usr/share/applications/nvtop.desktop

# create some useful script and link
ln -s /bin/fbterm /sbin/chshell
cat > /bin/nvrun << EOF
#!/bin/bash
__NV_PRIME_RENDER_OFFLOAD=1 __VK_LAYER_NV_optimus=NVIDIA_only __GLX_VENDOR_LIBRARY_NAME=nvidia "\$@"
EOF
chmod +x /bin/nvrun

# system configureation
	# enable to use framebuffer at tty
	usermod -a -G video root
	usermod -a -G video ${USER_NAME}

	# only save log file from latest 4 weeks
	journalctl --vacuum-time=4w

	# setting language
	echo "export LANGUAGE=en_US"	>> /home/${USER_NAME}/.bashrc
	echo "export LANG=en_US.UTF-8"	>> /home/${USER_NAME}/.bashrc

	# enable ${USER_NAME} using sudo
	usermod -a -G sudo ${USER_NAME}
	
	# battery action setting (for laptop poweroff when lower then 15% )
	if [ ! -a /etc/UPower/UPower.conf.old ]; then
		cp /etc/UPower/UPower.conf /etc/UPower/UPower.conf.old
		cat /etc/UPower/UPower.conf.old | \
			sed -e 's\PercentageLow=.*\PercentageLow=25.0\g' | \
			sed -e 's\PercentageCritical=.*\PercentageCritical=20.0\g' | \
			sed -e 's\PercentageAction=.*\PercentageAction=15.0\g' | \
			sed -e 's\CriticalPowerAction=.*\CriticalPowerAction=PowerOff\g' | \
			sed -e 's\AllowRiskyCriticalPowerAction=PowerOff\AllowRiskyCriticalPowerAction=false\g' | \
			grep -v "#" > /etc/UPower/UPower.conf
	fi
		
	# setting laptop lip closing action (don't suspend)
	if [ ! -a /etc/systemd/logind.conf.bak ]; then
		cp /etc/systemd/logind.conf /etc/systemd/logind.conf.bak
		cat /etc/systemd/logind.conf.bak | \
			sed -e 's/#HandleLidSwitch=suspend/HandleLidSwitch=ignore/g' > /etc/systemd/logind.conf
	fi

	
# user configuration
if [ -a vimrc ]; then
	cp vimrc /home/${USER_NAME}/.vimrc
fi
#dconf write /org/gnome/Ptyxis/Profiles/$(dconf read /org/gnome/Ptyxis/default-profile-uuid | sed -e 's/\x27//g')/opacity 0.9

echo OK
```

然后重启计算机，再安装NVIDIA官方驱动并重启计算机，最后安装Hanabi动态壁纸。  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.4 | Date: 2025-08-11
