# Vim tutorial

[TOC]

## Get help

+ `:help` read help document
+ `:help COMMAND` get help for COMMAND
+ `^W^W` switch from one window to another
+ `:q` quit help
+ `^D` or `Tab` see possible completions

## Configure Vim

Vim can be configured user-specificly by a `.vimrc` file:

```shell
vim ~/.vimrc

set number " show line numbers
syntax on " enable syntax highlighting
set tabstop=4 " set the tab size to 4
set softtabstop=4 " remove the number of white spaces that was replaced by for the tab
set autoindent " enable automatic indentation
set expandtab " replace tabs with white spaces
set cursorline " highlight the current line

set background=dark
filetype plugin indent on
set showmatch " Show matching brackets.
set ignorecase " Do case insensitive matching
set smartcase " Do smart case matching
set incsearch " Incremental search
set autowrite " Automatically save before commands like :next and :make
set hidden " Hide buffers when they are abandoned
set mouse=a " Enable mouse usage (all modes)

setlocal noswapfile " 不要生成swap文件
set bufhidden=hide " 当buffer被丢弃的时候隐藏它
colorscheme evening " 设定配色方案
set number " 显示行号
set cursorline " 突出显示当前行
set ruler " 打开状态栏标尺
set shiftwidth=4 " 设定 << 和 >> 命令移动时的宽度为 4
set softtabstop=4 " 使得按退格键时可以一次删掉 4 个空格
set tabstop=4 " 设定 tab 长度为 4
set nobackup " 覆盖文件时不备份
set autochdir " 自动切换当前目录为当前文件所在的目录
set backupcopy=yes " 设置备份时的行为为覆盖
set hlsearch " 搜索时高亮显示被找到的文本
set noerrorbells " 关闭错误信息响铃
set novisualbell " 关闭使用可视响铃代替呼叫
set t_vb= " 置空错误铃声的终端代码
set matchtime=2 " 短暂跳转到匹配括号的时间
set magic " 设置魔术
set smartindent " 开启新行时使用智能自动缩进
set backspace=indent,eol,start " 不设定在插入状态无法用退格键和 Delete 键删除回车符
set cmdheight=1 " 设定命令行的行数为 1
set laststatus=2 " 显示状态栏 (默认值为 1, 无法显示状态栏)
set statusline=\ %<%F[%1*%M%*%n%R%H]%=\ %y\ %0(%{&fileformat}\ %{&encoding}\ Ln\ %l,\ Col\ %c/%L%) " 设置在状态行显示的信息
set foldenable " 开始折叠
set foldmethod=syntax " 设置语法折叠
set foldcolumn=0 " 设置折叠区域的宽度
setlocal foldlevel=1 " 设置折叠层数为 1
nnoremap <space> @=((foldclosed(line('.')) < 0) ? 'zc' : 'zo')<CR> " 用空格键来开关折叠
```




## Mode

- **Normal** mode - used for editor commands. This is also the default mode, unless the `insertmode` option is specified.
- **Visual** mode - similar to normal mode, but used to highlight areas of text. Normal commands are run on the highlighted area, which for an instance can be used to move or edit a selection.
- **Select** mode - works similarly to visual mode. However, if a printable character, carriage return, or newline (or line feed) is entered, Vim inserts the character, and starts insert mode.
- **Insert** mode - similar to editing in most modern editors. In insert mode, buffers can be modified with the text inserted.
- **Command-line** or **Cmdline** mode - supports a single line input at the bottom of the Vim window. Normal commands (beginning with `:`), and some other specific letters corresponding to different actions (including pattern search and the filter command) activate this mode.
- **Ex** mode - similarly to Cmdline mode, it takes a single line input at the bottom of the window. However, in Cmdline mode, entering a command exits the mode when the command is executed. Entering a command in Ex mode doesn't cause the mode to change.

## Normal mode

### Navigation

+ `h` or `←` *cursor* left.
+ `j` or `↓` *cursor* down.
+ `k` or `↑` *cursor* up.
+ `l` or `→` *cursor* right.
+ `w` move until the start of the next *word*.
+ `e` move to the end of the current *word*.
+ `b` move to the start of the last *word*. 
+ `0` (zero) move to the start of the *line*.
+ `$` move to the end of the *line*.
+ `^F` or `Page Down` move to the next *page*.
+ `^B` or `Page Up` move to the last *page*.
+ `G` move to the bottom of the *file*.
+ `gg` move to the start of the *file*.
+ `<N>G` move to the N-th *line* of the file
+ `<N>Enter` move down N *lines*. 
+ `^G` show your location in the file and the file status.
+ `%` jump between paired *parentheses* () or {} or []. 

### Grammar:

+ `[N]`+`MOTION` do n times MOTION, like `3w` go forward 3 words , `10j` go down 10 lines.
+ `OPERATOR`+`[N]`+`MOTION`
+ `[N]` + `OPERATOR`

### Editing action

+ `d + [N] + MOTION` delete something.
+ `dd` delete a whole line. **Remember** to use Vim grammar, this is equal to `dj`, `dk`, `d1Enter`, `d$` and so on.
+ `x` delete a character.
+ `u` lowercase u, undo previous action.
+ `U` capital U, undo all the changes on a line.
+ `^R` undo the undo's.
+ `p` put back text that has just been deleted below the current cursor.
+ `r + <character>` replace the character at the cursor with `<character>`.
+ `c + [N] + MOTION` change something. Equal to delete something and enter Insert mode.
+ `y + MOTION` yank (copy) something.
+ `yy` yank the line where cursor is on, equal to `y$`.
+ `p` paste below the cursor.
+ `P` paste above the cursor.
+ `.` repeat the last action you have just done.

### Search and Substitute

+ `/ or ? + PHRASE` search for the `PHRASE`, `/` will searches FORWARD for the phrase, while `?` BACKWARD for the phrase.
  + `n` search the next one below.
  + `N` search the next one above.
+ `[#1,#2]|[%]:s/old/new[/param]` substitute (replace multiple characters, words and lines) new for the old
  + `[#1,#2]` stands for substitute phrases between two line #'s
  + `%` stands for substitute all in this file.
  + `/g` substitute all in a line
  + `/c` ask for confirmation each time

### Advance action

+ `:!+COMMAND` execute an external COMMAND
+ 

### Save and Quit

+ `:w` save
+ `:q` quit (no changes have been made to the file)
+ `:q!` or `ZQ` quit without saving (discard changes)
+ `:wq` or `ZZ` save and quit
+ `:[#1,#2]w FILENAME` writes the current contents to disk with name FILENAME.
+ `:r FILENAME` retrieves disk file FILENAME and puts it below the cursor position.
+ `v motion` saves the Visually selected lines in file FILENAME.

## Insert mode 

- `i` - insert text before cursor
- `I` - insert text before first *non-blank* character in same line
- `a` - insert text after cursor
- `A` - insert text at the end of line
- `o` - Start a new line below cursor
- `O` - start new line above cursor

## Visual mode

- `v` individual Visual mode
- `C-v` block-wise Visual mode
- `V` line-wise Visual mode