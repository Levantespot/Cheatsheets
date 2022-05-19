### bashrc

```bash
##### proxy list #####
# https://zhuanlan.zhihu.com/p/47849525
# https://zhuanlan.zhihu.com/p/153124468
# 注意不 export 的话，别的 bash 脚本是访问不到这个变量的，为了能在 .ssh/config 访问到，必须 export 一下
# https://unix.stackexchange.com/a/495163
export host_ip=$(cat /etc/resolv.conf |grep "nameserver" |cut -f 2 -d " ")
# wget 比较特殊，不认 all_proxy，只认 http_proxy 和 https_proxy
alias proxy="export all_proxy=http://$host_ip:7890 http_proxy=http://$host_ip:7890 https_proxy=http://$host_ip:7890"
alias unproxy='unset all_proxy http_proxy https_proxy'
proxy
```

### vimrc

```shell
set background=dark
filetype plugin indent on
set showmatch " Show matching brackets.
set ignorecase " Do case insensitive matching
set smartcase " Do smart case matching
set incsearch " Incremental search
set autowrite " Automatically save before commands like :next and :make
set hidden " Hide buffers when they are abandoned
" set mouse=a " Enable mouse usage (all modes)

setlocal noswapfile " 不要生成swap文件
set bufhidden=hide " 当buffer被丢弃的时候隐藏它
" colorscheme evening " 设定配色方案
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

### tmux.conf

First clone `tpm`:  `$ git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`

```shell
bind-key c new-window -c "#{pane_current_path}"
bind-key % split-window -h -c "#{pane_current_path}"
bind-key '"' split-window -c "#{pane_current_path}"
set -g default-terminal 'screen-256color'
set -g history-limit 10000

set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'

set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'
set -g @continuum-restore 'on'tmux

run '~/.tmux/plugins/tpm/tpm'
```

Installing plugins

1. Add new plugin to `~/.tmux.conf` with `set -g @plugin '...'`
2. `$ tmux source ~/.tmux.conf`
3. Press `prefix` + I (capital i, as in **I**nstall) to fetch the plugin.

You're good to go! The plugin was cloned to `~/.tmux/plugins/` dir and sourced.

Uninstalling plugins

1. Remove (or comment out) plugin from the list.
2. Press `prefix` + alt + u (lowercase u as in **u**ninstall) to remove the plugin.

All the plugins are installed to `~/.tmux/plugins/` so alternatively you can find plugin directory there and remove it.
