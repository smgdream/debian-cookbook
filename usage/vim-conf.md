# 配置Vim

默认的vim可能用着不太习惯，例如vim默认8格缩进且不显示行号，所以我们要配置vim令其符合我们的使用习惯。vim支持插件，根据需求安装插件后我们能获得更加完善更加舒适的开发体验。

## vim配置文件
系统全局的vim配置文件路径为：`/etc/vim/vimrc`，用户vim配置文件的路径为`~/.vimrc`（如果没有则新建一个）。

**常用配置**  
| 配置 | 效果 |
| --- | --- |
| `"comment` | 注释 |
| `syntax on` | 开启语法高亮 |
| `set tabstop=N` | 设置TAB缩进相当于N个空格 |
| `set number` | 显示行号 |
| `set autoindent` | 自动缩进（缩进与上一行保持一致） |
| `set showcmd` | 在底部显示当前键入的命令 |
| `set encoding=utf-8` | 使用UTF-8编码 |
| `set showmatch` | （默认）高亮与当前括号匹配的另一个括号 |
| `filetype on` | 自动检测文件类型 |
| `colorscheme THEME_NAME` | 使用名为THEME_NAME的主题 |
| `filetype indent no` | 自动检测文件类型使用相应缩进规则 |
| `filetype plugin on` | 自动检测文件类型使用相应插件 |

**更多关于vim配置项的资料源**  
https://ruanyifeng.com/blog/2018/09/vimrc.html
https://vimdoc.sourceforge.net/htmldoc/options.html

## vim插件的安装

### 通过vim plug安装和管理插件

#### 安装vim plug
**在线安装**  
```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```
**手动安装**  
确保存在目录`~/.vim/autoload/`，如果不存在则通过命令`mkdir -pv ~/.vim/autoload/`创建相应目录。然后在[vim plug发布页](https://github.com/junegunn/vim-plug/releases)下载最新版本。之后解压压缩包并将其中的`plug.vim`复制到`~/.vim/autoload/`目录中

**启用vim plug**  
在vim配置文件中写入一下内容：  
```
call plug#begin()

" List your plugins here

call plug#end()
```

#### vim plug的使用方法

在`call plug#begin()`和`call plug#end()`插入一行配置`Plug '插件github短路径'`，插件github短路径格式为：`插件仓库所属用户名/插件仓库名`（如NERDTree的github仓库为`https://github.com/preservim/nerdtree`那么其短路径为`preservim/nerdtree`），然后保存并退出vim配置文件，之后打开vim执行命令`:PlugInstall`即可安装对应插件。然后在常规模式下键入插件的执行命令即可使用对应插件。  

**示例：通过vim plug安装NERDTree**  
ERDTree的github仓库为`https://github.com/preservim/nerdtree`所以其github短路径为`preservim/nerdtree`，于是在`call plug#begin()`和`call plug#end()`插入一行配置`Plug 'preservim/nerdtree'`配置变为：  
```
call plug#begin()

Plug 'preservim/nerdtree'

call plug#end()
```
然后保存退出vim配置文件。之后vim执行命令`:PlugInstall`稍等一会即完成NERDTree的安装。后面执行NERDTree的使用命令`:NERDTree`即可使用NERDTree插件。

**如需删除插件** 则在vim配置文件中移除插件相应的Plug行，然后在vim中执行`:PlugClean`命令即可卸载相应插件。  

关于vim plug的详细说明见[vim-plug的github仓库](https://github.com/junegunn/vim-plug)。  

### 手动安装vim插件
通常插件存放在`~/.vim/pack`目录中，但直接将插件文件夹复制到该目录中是无法使用插件的，还需要在`~/.vim/pack`目录中创建插件组文件夹，插件组文件夹名可以随便设置一个（如：`foo`），然后还要在插件组文件夹中新建一个start目录。之后将插件目录复制到start目录中即可完成插件的安装。后面启动vim并输入插件使用命令即可执行插件。

**示例：手动安装NERDTree**  
如果没用`~/.vim/pack`目录则通过`mkdir -pv ~/.vim/pack`命令新建一个，然后在该目录中新建一个`foobar`命令，再在`foobar`目录中新建一个`start`目录。

在[NERDTree的tag页](https://github.com/preservim/nerdtree/tags)下载NERDTree插件，然后通过`tar -xvf /PATH/TO/NERDTREE_TARBALL_FILE`命令解压，然后将解压出来的项目文件夹重命名为`nerdtree`，随后通过`cp -r nerdtree ~/.vim/pack/foobar/start/`命令将`nerdtree`目录复制到`~/.vim/pack/foobar/start/`目录中。之后即可启动vim通过`:NERDTree`命令执行NERDTree。

### 自动使用插件
默认情况下安装插件后要进入vim并输入对应命令后才可以使用插件，这显然有点麻烦，用户可能希望进入vim后vim能自动执行插件。

在vim配置文件中添加以下内容即可在vim启动时自动执行插件。
```
autocmd vimenter * PLUGIN_COMMAND
```
注：`PLUGIN_COMMAND`为执行插件的命令，但不需要开头的冒号。

**扩展资料**  
自动命令的简单格式如下：
```
autocmd EVENTS PATTERN COMMAND
```
说明：  
`autocmd` 该配置项用于设置自动命令运行。  
`EVENTS` 该参数用于设置命令触发的事件，是触发命令的条件之一。（如：vimenter说明在启动vim时触发命令）  
`PATTERN` 该参数用于设置在满足EVENTS要求后，触发命令的文件匹配模式要求，是触发命令的条件之二。（如：`*`匹配所有文件，`*.c`则匹配所有后缀为.c的C源文件）  
`COMMAND` 为执行的命令，如果`autocmd EVENTS PATTERN COMMAND`是配置文件中的配置项则`COMMAND`不需要以`:`开头。  


## 参考资料

\[1\] [Vim 配置入门 - 阮一峰的网络日志](https://ruanyifeng.com/blog/2018/09/vimrc.html)  
\[2\] [从零开始配置 vim(8)——文件类型检测](https://zhuanlan.zhihu.com/p/550119288)  
\[3\] [Vim documentation: options](https://vimdoc.sourceforge.net/htmldoc/options.html)  
\[4\] [junegunn/vim-plug: :hibiscus: Minimalist Vim Plugin Manager](https://github.com/junegunn/vim-plug)  
\[5\] [preservim/nerdtree: A tree explorer plugin for vim.](https://github.com/preservim/nerdtree)  
\[6\] vim :help packages  
\[7\] [详谈Vim的配置层次结构与插件加载方式（一）](https://blog.csdn.net/qq_27825451/article/details/100518128)  
\[8\] [VIM学习笔记 自动命令(autocmd)](https://zhuanlan.zhihu.com/p/98360630)  
\[9\] [Autocommands / Learn Vimscript the Hard Way](https://learnvimscriptthehardway.stevelosh.com/chapters/12.html)  
\[10\] [使用 oh-my-bash 和 Nightfly 美化 Bash 和 Vim](https://cn.linux-console.net/?p=16191)  

---
Author: smgdream | License: CC BY-NC-SA 4.0 | Version: 0.5.1 | Date: 2025-08-16
