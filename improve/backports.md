# Backports源的配置与使用

**特别注意：使用Backports有少量风险，使用时需慎重一些。**  

在如今的互联网上也许你会看见这样一种论调：“Debian的软件太老了”，曾经的Debina软件确实如此，但现在已经得到了一定程度的改善。例如Gnome 48于2025-03-19发布而Debian Trixe于2025-04-15软冻结，而Debian团队愣是在这一个月不到的时间内把把Gnome 48塞进了Debian Trixie中。但是Debian以两年一次大更新的节奏更新Debian，而Debian是以稳定著称的，所以在一个大版本发布后的两年中仅会发布对已有软件进行稳定性修复和安全修复相关的更新。但两年又确实有点久了，即使Debian团队冻结前把许多新版本的软件塞进了Debian中，但过了两年不管怎么说软件也确实有些落伍了。  

而通常来说解决Debian稳定版软件旧的问题的方法有两种。一个是不使用Debian稳定版而使用Debian Testing（测试版）。Testing提供了较新的软件，在大部分时间中该版本都会不断的更新其中的软件，Testing可以算作是半个滚动式更新版。但正如其名，使用该版本可能会存在一些不稳定因素。而另一个方法便是使用Backports源。  

Backports源将Debian Testing中的部分软件下放到了稳定版中，故通过安装使用Backports源的软件我们就能使用部分较新版本的软件。下面笔者将说明如何配置Backports源并安装Backports源中提供的新版本软件。  

## Backports源的配置方法

创建文件`/etc/apt/sources.list.d/debian-backports.sources`并在其中写入以下内容。
```
Types: deb
URIs: http://deb.debian.org/debian
Suites: trixie-backports
Components: main contrib non-free non-free-firmware
Enabled: yes
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
```
注1：发行版名称(`Suites`)可能要根据实际情况稍作修改。在部分地区因为网络原因通过Debian主站更新软件较慢则可以将`URIs`对应的值替换为其他镜像站的地址（如：`https://mirrors.cernet.edu.cn/debian/`或`https://mirrors.ustc.edu.cn/debian/`）。  

注2：还存在一个特殊的Backports源称为Backports sloppy，该源用于将Testing的软件下放到old stable中。但使用Backports sloppy将破坏系统的可升级性。故本文不对其进行讨论也不建议用户使用，如果确实有使用该源的需要另见：[The Oldstable-sloppy Suite](https://backports.debian.org/Instructions/#index4h2)。  

然后执行`apt update`更新软件包列表。

## 通过Backports源安装软件

Backports源的优先级较低，所以即使Backports源软件是最新的，执行apt upgrade也不会自动将软件升级为Backports源中的版本。当然，这是故意这么设计的，**因为使用Backports源有破坏软件的依赖关系破坏系统稳定性的风险**
。所以想要使用Backports源中的软件需要手动更新/安装。方法如下:  

首先尝试执行以下命令看看是否能安装。
```sh
apt install PACKAGE_NAME/trixie-backports
```
注：对于不同代号的Debian则其中的`trixie-backports`需更改为对应代号的backports（如: `bookworm-backports`）

上面的命令将尝试仅安装/更新指定软件包，如果当前系统或非backports源中的依赖满足该新版本软件包的需求，命令可以成功执行。但如果无法满足Backports源中新软件的依赖需求则安装/更新失败，此时需要通过以下命令安装/更新软件。  
```sh
apt install -t trixie-backports PACKAGE_NAME
```
注：同上，对于不同代号的Debian，其中的`trixie-backports`需要更改为对应代号的backports。  
说明：该命令将安装指定软件包并优先从backports中查找所需依赖并安装。  

所以为什么不一步到位使用第二条命令呢？因为第二条命令会优先考虑安装backports中的依赖，这将会导致安装较多的Backports软件包。Backpotrs中的软件装多了，破坏系统稳定性的风险自然也就增加了。  

## 参考资料

\[1\] [Debian Backports](https://backports.debian.org/)  
\[2\] [Debian Backports ›› Instructions](https://backports.debian.org/Instructions/)  
\[3\] [Backports - Debian Wiki](https://wiki.debian.org/Backports)  
\[4\] [AptConfiguration - Debian Wiki](https://wiki.debian.org/AptConfiguration)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.7.1 | Date: 2025-10-24