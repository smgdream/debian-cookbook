# 日志的查看与管理

当应用程序或系统出现错误时我们可能需要查看日志中是否有相关的错误记录。当操作系统被长时间使用后日志文件可能会占用较大的空间我们可能还要删除部分日志。下面笔者将介绍日志的查看方法与管理方法。<!-- 此段落待调整 -->

## 使用journalctl查看和管理日志
查看日志的通用方法是使用journalctl程序，journalctl用于查看和管理systemd的日志条目。journalctl查看日志的常用命令如下：

| 命令 | 功能 |
| - | - |
| `journalctl` | 查看当前用户可查看的所有的日志 |
| `journalctl --user` | 查看当前用户的日志 |
| `journalctl --system` | 查看系统日志（需以root身份执行） |
| `journalctl -k` | 查看内核消息（需以root身份执行） |

注：默认情况下，打印的日志按时间升序排序。如需其降序排序则要添加参数`-r`用以将输出逆序。

journal的日志文件存放在`/var/log/journal`目录中。如需删除journal日志文件可使用`--vacuum-size=nU`参数保留最近的n个数量级代号U字节大小的内容（如16K、8M、4G），更旧日志则删除。除此之外，还可以使用`--vacuum-time=nT`删除最近n个时间单位T (s, m, h, d, w, m, y) 之外的内容，如`3d`将保留最近3天的内容，其他的则删除。

## 其他查看和管理日志的方法
除了使用journalctl查看日志，我们还可以使用其他命令查看各类日志。  
| 命令 | 功能 |
| - | - |
| `dmesg` | 查看内核缓冲环中的内核消息 |
| `wtmpdb last` | 查看系统的登录日志 |
| `history` | 查看曾经执行过的命令 |


我们亦可以进入日志目录以查看可阅读日志，如/var/log，许多程序将其日志保存在此目录中。

如果我们要删除日志可以使用`truncate -s 0 <file>`将日志正确地截断为0字节。  

\[1\] JOURNALCTL(1)  
\[2\] [systemd/Journal - ArchWiki](https://wiki.archlinux.org/title/Systemd/Journal  )  
\[3\] [Filesystem Hierarchy Standard 3.0](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.pdf)  
\[4\] [How to Clear System Log Files in Linux (2026)](https://www.fosslinux.com/111830/how-to-empty-or-clear-system-log-files-in-linux.htm)

---
Author: smgdream | Licnese: CC BY-NC-SA 4.0 | Version: 0.4 | Date: 2026-10-16