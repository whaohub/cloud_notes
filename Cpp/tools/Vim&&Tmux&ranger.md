# Vim

#### vim 显示行号

Vim 配置入门: https://www.ruanyifeng.com/blog/2018/09/vimrc.html

* 临时显示行号

```
a. 显示当前行行号，在vi命令行模式输入
:nu
b. 显示所有行号，在vi命令行模式输入
:set nu
```

* 永久显示行号

如果想让vim永久显示行号，则需要修改vim配置文件vimrc。如果没有此文件可以创建一个。在启动vim时，当前用户根目录下的vimrc文件会被自动读取，因此一般在当前用户的根目录下创建vimrc文件，即使用下面的命令：

```
vim ~/.vimrc
```

在打开的vimrc文件中最后一行输入：set number ，然后保存退出。再次用vim打开文件时，就会显示行号了

## Vim 配置

```shell
set showmatch
# 光标遇到圆括号、方括号、大括号时，自动高亮对应的另一个圆括号、方括号和大括号。

set hlsearch
# 搜索时，高亮显示匹配结果。

set incsearch
# 输入搜索模式时，每输入一个字符，就自动跳到第一个匹配的结果。

set ignorecase
# 搜索时忽略大小写。

set smartcase
# 如果同时打开了ignorecase，那么对于只有一个大写字母的搜索词，将大小写敏感；其他情况都是大小写不敏感。比如，搜索Test时，将不匹配test；搜索test时，将匹配Test。

set spell spelllang=en_us
# 打开英语单词的拼写检查。

set fileencodings=utf-8,gb2312,gb18030,gbk,ucs-bom,cp936,latin1
# 中文乱码问题

 set number
 set cursorline
 syntax on
 set showmode
 set encoding=utf-8

 " Search

set hlsearch
set showmatch
set incsearch
set ignorecase
set smartcase

 " Spell
 set spell spelllang=en_us   
```

## Vim 编码问题

```shell
一、存在3个变量：
encoding----该选项使用于缓冲的文本(你正在编辑的文件)，寄存器，Vim 脚本文件等等。\
这事可以把 'encoding' 选项当作是对 Vim 内部运行机制的设定。
fileencoding----该选项是vim写入文件时采用的编码类型。
termencoding----该选项代表输出到客户终端（Term）采用的编码类型。
二、此3个变量的默认值：

encoding----与系统当前locale相同，所以编辑文件的时候要考虑当前locale，否则要设置的东西就比较多了。
fileencoding----vim打开文件时自动辨认其编码，fileencoding就为辨认的值。\
为空则保存文件时采用encoding的编码，如果没有修改encoding，那值就是系统当前locale了。
termencoding----默认空值，也就是输出到终端不进行编码转换。
```

vim 编辑多行

```shell
Press Ctrl + v to enter into visual block mode.

Use ↑ / ↓ / j / k to select multiple lines.

Press Shift + i and start typing what you want.

After you press Esc, the text will be inserted into all the lines you selected.

Remember that Ctrl+c is not 100% equivalent to Esc and will not work in this situation!

There are slight variations of Shift + i that you can press while in visual block mode:

Key    Description
c or s    Delete selected block and enter insert mode
C    Delete selected lines (from cursor until end) and enter insert mode
R    Delete selected lines and enter insert mode
A    Append to selected lines, with the column at the end of the first line
Also note that pressing . after a visual block operation will repeat that operation where the cursor is!
```

## vim 编辑时无权限保存操作

```shell
:w !sudo tee %
```

[ref]: https://www.jb51.net/LINUXjishu/608817.html



## vim粘贴格错乱

esc:set paste进入paste 模式后，按i 键进入插入模式，然后再粘帖，文本**格式**不会错乱了

## vim 删除单词而不是剪切规避

当使用`p`命令粘贴时会默认粘贴" "寄存器中的内容，使用`dd`后寄存器中的内容被覆盖，这是可以使用yank的寄存器`“0`中粘贴 eg:

```shell
yy
dd
"0p
```

## Tmux 配置

```shell
  # 支持鼠标选取文本等
  # 支持鼠标拖动调整面板的大小(通过拖动面板间的分割线)
  # 支持鼠标选中并切换面板
  # 支持鼠标选中并切换窗口(通过点击状态栏窗口名称)
  set-option -g mouse on

  unbind '"'
  bind - splitw -v -c '#{pane_current_path}' # 垂直方向新增面板，默认进入当前目录
  unbind %
  bind | splitw -h -c '#{pane_current_path}' # 水平方向新增面板，默认进入当前目录

  # 绑定hjkl键为面板切换的上下左右键
  bind -r k select-pane -U # 绑定k为↑
  bind -r j select-pane -D # 绑定j为↓
  bind -r h select-pane -L # 绑定h为←
  bind -r l select-pane -R # 绑定l为→

  # 绑定Ctrl+hjkl键为面板上下左右调整边缘的快捷指令
  bind -r ^k resizep -U 10 # 绑定Ctrl+k为往↑调整面板边缘10个单元格
  bind -r ^j resizep -D 10 # 绑定Ctrl+j为往↓调整面板边缘10个单元格
  bind -r ^h resizep -L 10 # 绑定Ctrl+h为往←调整面板边缘10个单元格
  bind -r ^l resizep -R 10 # 绑定Ctrl+l为往→调整面板边缘10个单元格
 setw -g mode-keys vi # 开启vi风格后，支持vi的C-d、C-u、hjkl等快捷键
  bind Escape copy-mode # 绑定esc键为进入复制模式
  bind -T copy-mode-vi v send-keys -X begin-selection
  bind -T copy-mode-vi y send-keys -X copy-selection-and-cancel
  bind -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "xsel -i --clipboard"

  set -g default-terminal "tmux-256color" #设置终端颜色


  # List of plugins
  set -g @plugin 'tmux-plugins/tpm'
  set -g @plugin 'tmux-plugins/tmux-sensible'

  # Other examples:
  # set -g @plugin 'github_username/plugin_name'
  # set -g @plugin 'github_username/plugin_name#branch'
  # set -g @plugin 'git@github.com:user/plugin'
  # set -g @plugin 'git@bitbucket.com:user/plugin'
  # Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
  run '~/.tmux/plugins/tpm/tpm'
```

## tmux 自动保存会话

[服务器自动保存tmux会话以及恢复tmux会话 - 腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1990169?from=article.detail.1659864)

## neovim配置

插件

```shell
Plug 'christoomey/vim-tmux-navigator'
<ctrl-L> 切换到右侧窗口
<ctrl-H>切换到左侧窗口
<ctrl-J>切换到下方窗口
<ctrl-K>切换到上方窗口
<ctrl-/>切换到刚刚的窗口
```

## telescope.nvim

文件搜索插件 https://github.com/nvim-telescope/telescope.nvim

```
依赖
sudo apt install fd-find
sudo apt install ripgrep


Plug 'nvim-lua/plenary.nvim'
Plug 'nvim-telescope/telescope.nvim', { 'tag': '0.1.1' }
```

## ranger 基本配置

### ranger 基本安装

```shell
$ sudo apt-get install ranger     # ranger 的主程序
$ sudo apt-get install caca-utils # img2txt 图片
$ sudo apt-get install highlight  # 代码高亮
$ sudo apt-get install atool　    # 存档预览
$ sudo apt-get install w3m        # html页面预览
$ sudo apt-get install poppler    # pdf预览
$ sudo apt-get install mediainfo  # 多媒体文件预览
```

## **三、预览配置**

首先需要下载能够将对应格式的文件转化为txt的程序：

```text
$ sudo apt-get install catdoc     # doc预览
$ sudo apt-get install docx2txt   # docx预览
$ sudo apt-get install xlsx2csv   # xlsx预览
```

**https://www.52gvim.com/post/ranger-tool-usage**
