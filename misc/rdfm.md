# 呐呐呐，你不会到现在还不会读手册吧？
没想到你声称自己是个Linux用户却连阅读手册(Manual)这种基本技能都不会，真是个杂鱼~那就让我教教你这条杂鱼怎么阅读手册吧。  
<!-- 待更杂鱼化 -->

`man`命令用于查看手册，使用man查看一个对象(item)的手册页面(page)的通用语法如下：
```
man [man options] [[section] page ...] ...
```
最常用的形式为`man page`，其中page指的是你要看的对象（的手册页面）。例如我们可以执行命令`man dd`来查看`dd`工具的手册。  

有时候我们会遇到类型不同名字一样对象，如命令printf和库函数printf。为了让man正确打开我们想要查看的printf对象的手册页我们还应额外指定对象所在的节号(section)。man-pages(7)<sup>注1</sup>约定俗成地定义了以下数节：  

| 节号 | 节名 | 描述<sup>注2</sup> |
| :----: | ---- | ---- |
| 1    | 用户命令（程序） | 能够被用户在shell中执行的命令，如dd、kill、date |
| 2    | 系统调用(System calls) | 包装了内核执行的操作的C函数，如execve、write、mmap |
| 3    | 库调用(Library calls) | 除了系统调用包装函数之外的所有库函数，其中有许多的libc库函数 |
| 4    | 特殊文件（设备） | /dev目录中能通过内核访问设备的文件，如zero、fuse、tty |
| 5    | 文件格式与配置文件 | 描述各种各样的文件格式和配置文件 |
| 6    | 游戏 | 游戏与有意思的小型程序 |
| 7    | 概述、惯例和杂项 | 各种话题(topics)的概述、惯例(conventions)和协议(protocols)的描述、一些标准(standards)及杂七杂八的东西 |
| 8    | 系统管理命令 | 例如mount之类的命令，此类命令能或只能由超级用户执行 |
| 9    | 内核例程（非标准节） | 内核例程的描述，如strscpy |

据此如需精确查阅某节的某个对象的手册页面可以使用形式为`man section page`<sup>注3</sup>的命令，其中section是节号。例如可以通过命令`man 3 printf`来查询库函数printf的手册。

注1：在此遵循手册格式的惯例，使用`page(section)`的形式表示第section节的"page"对象手册（页面）。

注2：对于1-8节，每节中都有一个intro对象用于介绍该节。

注3：`man section page`还存在两种等效形式：`man page.section`和`man 'page(section)'`。

在存在同名对象的情况下，如果没有指定节号则会匹配节号最小的符合名字的对象。例如执行`man printf`则会显示第一节（用户命令/程序）的printf命令，即`printf(1)`。

man命令还可以一次性查阅多个手册，如`man dd 3 printf`命令将显示dd(1)和printf(3)的手册页面。在使用形如`man page1 section page2 page3 ...`的命令查阅多个手册时section将会限定在其之后的page的查找范围，若要查询其他节的page则要再指定其所在的节号。如命令`man 3 printf malloc 2 time`将查阅printf(3)、malloc(3)及time(2)。如果在相应的节中未找到名字符合的对象则会提示未找到相应条目。

在一次性查阅多个手册时，man工具不是将手册拼接显示，而是逐个显示。在看完一个手册退出后man将询问用户是否要看下一项，然后根据用户的选择执行相应的操作。

**浏览手册页面的基本操作**非常简单，在进入手册页面后可以通过`k` `j`键、上下方向键、`page up` `page down`键及鼠标滚轮上下滚动页面，浏览完毕后可通过`q`键退出。

手册通常按照一定的格式与段落编排惯例进行编写，一个手册中通常会包含如下段落：

| 段落名 | 描述 |
| -------| ---- |
| NAME | 说明对象的名字并简要地说明对象的用途 |
| SYNOPSIS（概要） | 命令或函数接口的简要说明，即命令语法或函数原型 |
| DESCRIPTION | 功能或行为的描述 |
| EXAMPLES | 用法示例 |
| SEE ALSO | 相关资料 |

除此之外，依据描述对象的需求，其手册中还可能会有其他的段落，如LIBRARY（库）、OPTIONS（选项）、RETURN VALUE（返回值）、ERRORS（错误）、BUGS、AUTHOR等段落。

man程序还提供了一些选项用于控制其功能和行为。如`--help`参数可用于查看man命令的帮助信息。而`-a`选项则会让man显示所有匹配的对象的手册。例如命令`man -a printf`将令man显示printf(1)和printf(3)的手册。

如果想要将手册保存为文本文件则可以通过重定向来实现，例如`man section page >textfile`。要注意，终端宽度将决定输出文本中每行的换行位置和排版。


## 一些成册的手册资料

- [GNU+Linux Programmer's Manual](https://mirrors.edge.kernel.org/pub/linux/docs/man-pages/book/) 
- [Portable Operating System Interface (POSIX™) Base Specifications, Issue 8](https://standards.ieee.org/ieee/1003.1/7700/)

## 手册页示例
### cat(1)
```
CAT(1)                       User Commands                       CAT(1)

NAME
       cat - concatenate files and print on the standard output

SYNOPSIS
       cat [OPTION]... [FILE]...

DESCRIPTION
       Concatenate FILE(s) to standard output.

       With no FILE, or when FILE is -, read standard input.

       -A, --show-all
              equivalent to -vET

       -b, --number-nonblank
              number nonempty output lines, overrides -n

       -e     equivalent to -vE

       -E, --show-ends
              display $ at end of each line

       -n, --number
              number all output lines

       -s, --squeeze-blank
              suppress repeated empty output lines

       -t     equivalent to -vT

       -T, --show-tabs
              display TAB characters as ^I

       -u     (ignored)

       -v, --show-nonprinting
              use ^ and M- notation, except for LFD and TAB

       --help display this help and exit

       --version
              output version information and exit

EXAMPLES
       cat f - g
              Output  f's  contents, then standard input, then g's con‐
              tents.

       cat    Copy standard input to standard output.

AUTHOR
       Written by Torbjorn Granlund and Richard M. Stallman.

REPORTING BUGS
       GNU coreutils online  help:  <https://www.gnu.org/software/core‐
       utils/>
       Report   any   translation   bugs   to  <https://translationpro‐
       ject.org/team/>

SEE ALSO
       tac(1)

       Full documentation <https://www.gnu.org/software/coreutils/cat>
       or available locally via: info '(coreutils) cat invocation'

       Packaged by Debian (9.7-3)
       Copyright © 2025 Free Software Foundation, Inc.
       License GPLv3+: GNU GPL version 3 or later  <https://gnu.org/li‐
       censes/gpl.html>.
       This  is  free software: you are free to change and redistribute
       it.
       There is NO WARRANTY, to the extent permitted by law.

GNU coreutils 9.7              June 2025                         CAT(1)
```

### cos(3)
```
cos(3)                  Library Functions Manual                 cos(3)

NAME
       cos, cosf, cosl - cosine function

LIBRARY
       Math library (libm, -lm)

SYNOPSIS
       #include <math.h>

       double cos(double x);
       float cosf(float x);
       long double cosl(long double x);

   Feature    Test    Macro    Requirements   for   glibc   (see   fea‐
   ture_test_macros(7)):

       cosf(), cosl():
           _ISOC99_SOURCE || _POSIX_C_SOURCE >= 200112L
               || /* Since glibc 2.19: */ _DEFAULT_SOURCE
               || /* glibc <= 2.19: */ _BSD_SOURCE || _SVID_SOURCE

DESCRIPTION
       These functions return the cosine of x, where x is given in  ra‐
       dians.

RETURN VALUE
       On success, these functions return the cosine of x.

       If x is a NaN, a NaN is returned.

       If  x  is positive infinity or negative infinity, a domain error
       occurs, and a NaN is returned.

ERRORS
       See math_error(7) for information on how to determine whether an
       error has occurred when calling these functions.

       The following errors can occur:

       Domain error: x is an infinity
              errno is set to EDOM (but see BUGS).  An  invalid  float‐
              ing-point exception (FE_INVALID) is raised.

ATTRIBUTES
       For  an  explanation  of the terms used in this section, see at‐
       tributes(7).
       ┌────────────────────────────────────┬───────────────┬─────────┐
       │ Interface                          │ Attribute     │ Value   │
       ├────────────────────────────────────┼───────────────┼─────────┤
       │ cos(), cosf(), cosl()              │ Thread safety │ MT-Safe │
       └────────────────────────────────────┴───────────────┴─────────┘

STANDARDS
       C11, POSIX.1-2008.

HISTORY
       C89, POSIX.1-2001.

       The variant returning double also conforms to SVr4, 4.3BSD.

BUGS
       Before glibc 2.10, the glibc implementation did not set errno to
       EDOM when a domain error occurred.

SEE ALSO
       acos(3), asin(3), atan(3), atan2(3), ccos(3), sin(3), sincos(3),
       tan(3)

Linux man-pages 6.9.1          2024-05-02                        cos(3)
```


## 参考资料
\[1\] man(1)  
\[2\] [GNU+Linux Programmer's Manual 6.19](https://mirrors.edge.kernel.org/pub/linux/docs/man-pages/book/man-pages-6.19.pdf) man-pages(7), intro(2-8), strscpy(9)  
\[3\] Portable Operating System Interface (POSIX™) Base Specifications, Issue 8 Vol 1: Base Definition Capter 14 Headers, Vol 2: System Interfaces Section 1.2 Format of Entries, Vol 3: Shell and Utilities Section 1.4 Utility Description Defaults  
\[4\] [How to Read Manual Pages in Linux | man Command - GeeksforGeeks](https://www.geeksforgeeks.org/linux-unix/man-command-in-linux-with-examples/)  
\[5\] [man page - Wikipedia](https://en.wikipedia.org/wiki/Man_page)  
\[6\] [Linux Finally Eliminates The strncpy API After Six Years Of Work, 360+ Patches - Phoronix](https://www.phoronix.com/news/Linux-7.2-Drops-strncpy)  
\[7\] [Merge tag 'strncpy-removal-v7.2-rc1' of git.kernel.org/pub/scm/linux/kernel/git/kees/linux](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=1a3746ccbb0a97bed3c06ccde6b880013b1dddc1)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.4.9u | Date: 2026-09-13